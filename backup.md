---

copyright:
  years: 2019
lastupdated: "2019-01-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Backing up and restoring data
{: #backup-restore}

Use the following procedures to back up and restore user data in your {{site.data.keyword.dcp_short}} instance.

The procedures on this page are for advanced users who have experience administering IBM Cloud Private clusters. Backing up and restoring data does not need to be performed as part of standard use or maintenance.
{: important}

## General prerequisites
{: #gen-prereqs}

### Required tools
{: #tools}

Before backing up or restoring data, ensure that you have the following tools installed on your machine:

  - `etcdctl`, which is available at [https://github.com/etcd-io/etcd/tree/master/etcdctl ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/etcd-io/etcd/tree/master/etcdctl){:new_window}.
  - `elasticdump`, which is available at [https://www.npmjs.com/package/elasticdump ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.npmjs.com/package/elasticdump){: new_window} or, if you already have Node.js installed, by using the `npm install elasticdump` command.
  - `kubectl`, which is available as described at [Installing the Kubernetes CLI (kubectl) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.1/manage_cluster/install_kubectl.html){: new_window}.
  - `postgres`, which is available at [https://www.postgresql.org/download/ ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.postgresql.org/download/){: new_window}.
  
### Required permissions
{: #perms}
  
You must also have the following permissions:

  - Administrative access to the {{site.data.keyword.dcp_short}} instance on your IBM Cloud Private cluster whose data is to be backed up, as well as to the new instance to which the data is to be restored.
  - Credentials for the following data services:
    - ElasticSearch
    - `etcd`
    - `postgres`
  - Permissions to use cluster-wide CLI tools such as `kubectl`.
  
### Example syntax
{: #example-syntax}

In the following procedures, angle brackets (`< >`) are used to indicate variables that need to be substituted with values for your local installation, and curly brackets (`{ }`) are used literally in `bash` scripts and commands. This differs from the standard Watson documentation convention of using curly brackets to indicate substitutions.
{: important}

## Process overview
{: #overview}

The process can be summarized as follows:

### Backing up

  1. Ensure that the {{site.data.keyword.dcp_short}} instance being backed up does not have any incoming requests and that there are no outstanding in-flight requests.
  1. Obtain credentials for ElasticSearch, `etcd,` and `postgres`.
  1. For each data service, use the `kubectl port-forward` command to connect one of the service's local ports to a new port on another pod.
  1. Copy the data from the data service to the matching service on the new pod.
  1. Clean or groom the data as necessary.
  1. Give the backed-up data a new release name.

### Restoring

  1. Deploy a new {{site.data.keyword.dcp_short}} instance to the IBM Cloud Private cluster.
  1. For each data service, use the `kubectl port-forward` command to connect one of the service's ports on the existing pod to the matching data service on the new {{site.data.keyword.dcp_short}} instance.
  1. Copy the data from the new pod to the data service.
  1. Enable requests on the new {{site.data.keyword.dcp_short}} instance.
  
## Backup prerequisites
{: #backup-prereqs}

Before initiating a backup, observe the following requirements:

  - Ensure there are no incoming requests to the instance.
  - Ensure that any in-flight requests to the instance have been handled.
  - Ensure the ingestion queues are empty.
  - Back up all three data services in a short period of time to prevent the data from becoming inconsistent. For example, adding a new collection between backing up data services can cause {{site.data.keyword.dcp_short}} component inconsistencies.
  {: important}
  
There is no precise order in which the data services must be backed up or restored, provided no in-flight actions are permitted during the backup period. Queries are read-only operations and are therefore permitted during the backup period.
{: note}

## Backing up and restoring the Postgres service
{: #backup-restore-postgres}

Perform the following steps to back up and restore the `postgres` service.

### Backing up the Postgres service
{: #postgres-backup}

  1. Ensure that `postgres` is running on your {{site.data.keyword.dcp_short}} instance.
  1. Ensure that you have installed `postgres` as described in [Required tools](#tools) and that you have access to the `pg_dumpall` command.
  1. Ensure there are no processes listening to port `5432` by running the following command.
    ```bash
    lsof -i:5432
    ```
    {: pre}
    If the command returns with no output, there are no processes listening to the port. If the command returns with output, kill any process listening to the port by running the following command.
    ```bash
    kill $(lsof -i:5432)
    ```
    {: pre}
  1. Copy the following script and save it as `pgBackup.sh`.

    ```bash
    #! /bin/sh

    prev_deployment_name='<prev_deployment_name>'
    new_deployment_name='<new_deployment_name>'

    print_usage() {
        printf "Usage: ...\\n"
    }

    while getopts 'p:n:' flag; do
        case "${flag}" in
            p) prev_deployment_name="${OPTARG}" ;;
        n) new_deployment_name="${OPTARG}" ;;
        *) print_usage
           exit 1 ;;
        esac
    done

    if [ -z "${prev_deployment_name}" ]; then
        echo "Previous helm deployment name must be specified"
        print_usage
        exit 1
    fi

    if [ -z "${new_deployment_name}" ]; then
        echo "New helm deployment name must be specified"
        print_usage
        exit 1
    fi

    echo "Retrieving username and password for the postgres deployment from the following helm deployment: ${prev_deployment_name}"
    export PGUSER=`kubectl get secret ${prev_deployment_name}-postgres-discovery-authsecret -o jsonpath="{.data.pg_su_username}" | base64 --decode`
    export PGPASSWORD=`kubectl get secret ${prev_deployment_name}-postgres-discovery-authsecret -o jsonpath="{.data.pg_su_password}" | base64 --decode`
    export PGHOST="localhost"

    echo "Setting up port-forward to postgres discovery service"
    kubectl port-forward svc/${prev_deployment_name}-postgres-discovery 5432:5432 &
    PORT_FWD_PID=$!

    echo "Waiting for port-forward to be stable"
    sleep 5

    echo "Running pg_dumpall to save dump contents to a file"
    pg_dumpall -v -a -f pgDumpOutput

    kill $PORT_FWD_PID

    echo "\nKeeping only values from pgDump that need to be restored, and using new helm deployment name for restore"
    export prev_dep_name_underscore=`echo $prev_deployment_name | sed 's/-/_/g'`
    export new_dep_name_underscore=`echo $new_deployment_name | sed 's/-/_/g'`

    sed -e '/^COPY public\.clusters /,/^\\\./d' \
        -e '/^COPY public\.databasechangelog /,/^\\\./d' \
        -e '/^COPY public\.environments /,/^\\\./d' \
        -e '/^COPY public\.databasechangeloglock /,/^\\\./d' \
        -e '/^COPY public\.users /,/^\\\./d' \
        -e "s/${prev_dep_name_underscore}/${new_dep_name_underscore}/g" pgDumpOutput > outputToRestore
    ```
    {: codeblock}
    
    In lines 3 and 4 of the script, set the appropriate values for `<prev_deployment_name>` and `<new_deployment_name>`. Note that you provide the same values when running the script in Step 6.
  1. Make the script executable by running the following command:
     ```bash
     chmod +x pgBackup.sh
     ```
     {: pre}
  1. Run the `pgBackup.sh` script and specify the name of the original Postgres deployment you are backing up and the name of the new Postgres deployment to which you are restoring the data:
     ```bash
     ./pgBackup.sh -p <prev_deployment_name> -n <new_deployment_name>
     ```
     {: pre}
  1. The script generates a `pgDumpOutput` file in the current directory. The `pgDumpOutput` file contains the abbreviated output from the Postgres `pg_dumpall` command and an `outputToRestore` file that contains only the values from the data dump that need be restored.
    
### Restoring the Postgres service
{: #restore-postgres}

  1. Ensure that the local Postgres instance is running on `localhost` post `5432` by running the following command. This enables the `psql` command to connect to it.
     ```bash
     kubectl port-forward svc/<helm_release_name>-postgres-discovery 5432:5432
     ```
     {: pre}
     where `<helm_release_name>` is the release name of the Postgres deployment that was backed up.
  1. Modify the `PGUSER` and `PGPASSWORD` environment variables for the new Postgres deployment by running the following commands.
     ```bash
     export PGUSER=`kubectl get secret <new_deployment_name>-postgres-discovery-authsecret -o jsonpath="{.data.pg_su_username}" | base64 --decode`
     ```
     {:pre}
     ```bash
     export PGPASSWORD=`kubectl get secret <new_deployment_name>-postgres-discovery-authsecret -o jsonpath="{.data.pg_su_password}" | base64 --decode`
     ```
     {:pre}
     where `<new_deployment_name>` is the name of the new Postgres deployment to which the backed-up data is to be restored.
  1. Run the following command to restore the data to the new Postgres deployment:
     ```bash
     psql postgres < outputToRestore
     ```
     {: pre}

## Backing up and restoring the ElasticSearch service
{: #backup-restore-es}

Perform the following steps to back up and restore the ElasticSearch service.

### Backing up the ElasticSearch service
{: #es-backup}

  1. Install the `elasticdump` tool as described in [Required tools](#tools). The following command assumes you have `npm` (Node.js package manager) already installed.
     ```bash
     npm install elasticdump
     ```
     {: pre}
  1. Create a local directory in which to store the data backup:
     ```bash
     mkdir elastic-backup
     ```
     {: pre}
  1. Get the ElasticSearch credentials for your instance from Kubernetes by running the following command.
     ```bash
     export ESAUTH=$(kubectl get secret <prev_deployment_name>-elastic-discovery-auth -o jsonpath="{['data']['credentials\.txt']}" | base64 --decode | base64 --decode)
     ```
     where `<prev_deployment_name>` is the name of the ElasticSearch deployment that is being backed up.
  1. In a separate terminal session, run the following command to forward a port from your IBM Cloud Private cluster to your local machine:
     ```bash
     kubectl port-forward svc/<prev_deployment_name>-elastic-discovery 2443:443 & PORT_FWD_PID=$!
     ```
     where `<prev_deployment_name>` is the name of the ElasticSearch deployment that is being backed up.
  1. In the terminal session in which you created the `elastic-backup` directory and obtained ElasticSearch credentials, run the following command to back up the ElasticSearch data.
     ```bash
     ./node_modules/.bin/multielasticdump --direction=dump --input=https://$ESAUTH@localhost:2443/ --output=./elastic-backup
     ```
     {: pre}
  If you encounter a self-signed certificate error, set the following environment variable and run the previous command again.
     ```bash
     export NODE_TLS_REJECT_UNAUTHORIZED=0
     ```
     {: pre}
  1. In the terminal session in which you forwarded a port from your IBM Cloud Private cluster to your local machine, close the forwarded port by running the following command.
     ```bash
     kill $PORT_FWD_PID 
     ```

### Restoring the ElasticSearch service
{: #es-restore}

  1. Get the ElasticSearch credentials for your new instance from Kubernetes by running the following command.
     ```bash
     export ESAUTH=$(kubectl get secret <new_deployment_name>-elastic-discovery-auth -o jsonpath="{['data']['credentials\.txt']}" | base64 --decode | base64 --decode)
     ```
     where `<new_deployment_name>` is the name of the new ElasticSearch deployment to which you want to restore the data.
  1. Run the following command to forward a port from your IBM Cloud Private cluster to your local machine:
     ```bash
     kubectl port-forward svc/<new_deployment_name>-elastic-discovery 2443:443 & PORT_FWD_PID=$!
     ```
     where `<new_deployment_name>` is the name of the new ElasticSearch deployment to which you want to restore the data.
  1. Restore the data from the `elastic-backup` directory on your local machine to the new ElasticSearch deployment by running the following command.
     ```bash
     ./node_modules/.bin/multielasticdump --direction=load --input=./elastic-backup --output=https://$ESAUTH@localhost:2443/
     ```
     {: pre}
  1. Close the forwarded port by running the following command.
     ```bash
     kill $PORT_FWD_PID 
     ```

## Backing up and restoring the etcd service
{: #etcd-backup-restore}

{{site.data.keyword.dcp_short}} uses the `etcd` serivce to store hostnames and ports for microservice location. The values are overwritten with new values when a new release is deployed. There might be circumstances in which a manual change is made to the values stored in `etcd`. The following procedure documents the original values before the original deployment is removed.

Perform the following steps to back up and restore the `etcd` service.

  1. Ensure that you have the `etcdctl` tool installed as described in [Required tools](#tools). The tool is installed with every `etcd` deployment.
  1. Copy the following script and save it as `etcdShow.sh`.
     ```bash
     #! /bin/sh

     deployment_name='<deployment_name>'
     secret_name='etcd-authsecret'

     print_usage() {
       printf "Usage:\\n -d\\thelm deployment name of discovery release\\n -s\\tname of the kubernetes secret containing the etcd username and password. Default should suffice.\\n"
        }

     while getopts 'd:o:s:' flag; do
        case "${flag}" in
            d) deployment_name="${OPTARG}" ;;
            s) secret_name="${OPTARG}" ;;
            *) print_usage
               exit 1 ;;
        esac
     done

     if [ -z "${deployment_name}" ]; then
        echo "Deployment name must be specified using -d flag"
        print_usage
        exit 1
     fi

     echo "Retrieving username and password for the etcd deployment, from helm deployment ${deployment_name}"
     etcd_username="$(kubectl get secret ${deployment_name}-${secret_name} -o jsonpath='{.data.username}' | base64 --decode)"
     etcd_password="$(kubectl get secret ${deployment_name}-${secret_name} -o jsonpath='{.data.password}' | base64 --decode)"

     export ETCDCTL_USER="${etcd_username}:${etcd_password}"
     export ETCDCTL_INSECURE_SKIP_TLS_VERIFY=true
     export ETCDCTL_INSECURE_TRANSPORT=false
     export ETCDCTL_API=3
     export ETCDCTL_ENDPOINTS="localhost:2379"

     echo "Setting up port-forward to etcd service"
     kubectl port-forward svc/"${deployment_name}"-etcd 2379:2379 & PORT_FWD_PID=$!

     echo "Waiting for port-forward to be stable"
     sleep 10

     echo "Running etcdctl to print all keys & values. Save the output between the 'fences':"
     echo "----------------------------------"
     etcdctl get --prefix /
     echo "----------------------------------"

     kill $PORT_FWD_PID
     ```
     {: codeblock}
     
     In line 3 of the script, be sure to set the appropriate value for `<deployment_name>`.
  1. Make the script executable by running the following command.
     ```bash
     chmod +x etcdShow.sh
     ```
     {: pre}
  1. Run the script and save the output to a text file.
     ```bash
     ./etcdShow.sh > etcdShow.txt
     ```
     The script prints all of the keys and values contained in `etcd` to a text file named `etcdShow.txt` in the current directory. There is a maximum of approximately 15 keys in `etcd`, so the output of the script is short and can be used for later reference if you need to reset previous values after creating the new `etcd` deployment.






