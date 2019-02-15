---

copyright:
  years: 2018, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:note: .note}
{:important: .important}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Installing Watson Assistant Discovery Extension
{: #install}

Learn how to install the {{site.data.keyword.dcp_short}} service into {{site.data.keyword.icpfull}}.
{: shortdesc}

The {{site.data.keyword.icpfull_notm}} environment is a Kubernetes-based container platform that can help you quickly modernize and automate workloads that are associated with the applications and services you use. You can develop and deploy on your own infrastructure and in your data center which helps to mitigate risk and minimize vulnerabilities.

## Software requirements
{: #prereqs}

- {{site.data.keyword.icpfull_notm}} 3.1.0
- Kubernetes 1.11 or later
- Helm 2.9.1 or later

Before installing Watson Assistant Discovery Extension, _you must install and configure `helm` and `kubectl`_.

## Browser support and prerequisites
{: #browser_reqs}

For the list of {{site.data.keyword.icpfull_notm}} prerequisites and supported browsers, see [System requirements ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/supported_system_config/system_reqs.html){: new_window}.

## System requirements
{: #sys-reqs}

In addition to the [general hardware requirements and recommendations ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/supported_system_config/hardware_reqs.html){: new_window}, {{site.data.keyword.dcp_short}} has the following requirements:

  - **Architecture**: x86 is currently the only supported architecture
  - **Virtual private CPUs (VPCs)**: 90 VPCs (90 cores, 8 cores per node)
  - **RAM**: 128 GB RAM per 90 VPCs (90 cores)
  - **Nodes**: Eight (8) worker nodes per 90 VPCs (90 cores)
  - **Hard disk**: 768 Gi of storage space required per worker node (dependent on number and complexity of documents)
  - **Local storage** on [persistent volumes](#create-pvs)

## Resource requirements
{: #resource-rqts}

| Component | Number of replicas | Disk space per pod | Storage type  |
|-----------|--------------------|--------------------|---------------|
| Elastic   |                  3 |           10.00 Gi | local-storage |
| Etcd      |                  3 |            0.25 Gi | local-storage |
| Postgres  |                  2 |           16.50 Gi | local-storage |
| RabbitMQ  |                  1 |            0.25 Gi | local-storage |
| Redis     |                  2 |                    |               | 

### RBAC

By default the chart automatically installs the necessary RBAC roles and rolebindings to run the service. Outside of this, it is necessary that the _cluster administrator_ configures the `default` service account to have access to the following resources within the namespace where the chart will be installed: `jobs, jobs/status, serviceaccounts,roles, rolebindings, pods, secrets, configmaps, persistentvolumeclaims`. Moreover, the `default` service account needs permission to perform the following actions on the aforementioned resources: `get list delete`. Proper access to these resources will ensure no vestiges of the chart after uninstalling.

### Overview of the steps

1.  [Download service installation artifacts](#download-wade-icp)
1.  [Prepare the cloud environment](#install-icp)
1.  [Add the service chart to the cloud repository](#add-wade-chart-to-icp)
1.  [Install the service](#install-wade-from-catalog)
1.  [Verify that the installation was successful](#verify)
1.  [Launch the tool](#launch-tool)

## Step 1: Purchase and download installation artifacts
{: #download-wade-icp}

1.  Purchase {{site.data.keyword.dcp_short}} for {{site.data.keyword.icpfull_notm}} from [Passport Advantage ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/software/passportadvantage/index.html){:new_window}.

1.  Download the appropriate package for your environment.

    {{site.data.keyword.dcp_short}} for {{site.data.keyword.icpfull_notm}} includes {{site.data.keyword.icpfull_notm}} Foundation version 3.1.0.

    If you have {{site.data.keyword.icpfull_notm}} version 3.1.0 set up in your environment already, you can download the archive for {{site.data.keyword.dcp_short}} alone.

    If you do not have {{site.data.keyword.icpfull_notm}} or have a later major version of {{site.data.keyword.icpfull_notm}} set up in your organization (4.x, for example), you must install version 3.1.0. To get the {{site.data.keyword.icpfull_notm}} 3.1.0 archive included, choose the e-assembly that includes {{site.data.keyword.icpfull_notm}}.

The Passport Advantage archive (PPA) file for {{site.data.keyword.dcp_short}} contains a Helm chart and images. Helm is the Kubernetes native package management system that is used for application management inside an {{site.data.keyword.icpfull_notm}} cluster.

## Step 2: Prepare the cloud environment
{: #install-icp}

You must have cluster administrator or team administrator access to the systems in your cluster.

1.  If you do not have {{site.data.keyword.icpfull_notm}} version 3.1.0 set up, install it. See [Installing a standard IBM Cloud Private environment ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/installing/install_containers.html).
1.  Synchronize the clocks of the client computer and the nodes in the IBM Cloud Private cluster. To synchronize your clocks, you can use network time protocol (NTP). For more information about setting up NTP, see the user documentation for your operating system.
1.  If you have not done so, install the IBM Cloud Private command line interface and log in to your cluster. See [Installing the IBM Cloud Private CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_cluster/install_cli.html).
1.  Configure authentication from your computer to the Docker private image registry host and log in to the private registry. See [Configuring authentication for the Docker CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_images/configuring_docker_cli.html).
1.  If you are not a root user, ensure that your account is part of the `docker` group. See [Post-installation steps ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/installation/linux/linux-postinstall/#manage-docker-as-a-non-root-user) in the Docker documentation.
1.  Ensure that you have a stable network connection between your computer and the cluster.
1.  Install the Kubernetes command line tool, `kubectl`, and configure access to your cluster. See [Accessing your cluster from the kubectl CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_cluster/cfc_cli.html).
1.  Obtain access to the boot node and the cluster administrator account, or request that someone with that access level create your certificate. If you cannot access the cluster administrator account, you need an {{site.data.keyword.icpfull_notm}} account that is assigned to the operator or administrator role for a team and can access the kube-system namespace.
1.  Set up the Helm command line interface. See [Setting up the Helm CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/app_center/create_helm_cli.html) for details.

## Step 3: Add the Helm chart to the cloud repository
{: #add-wade-chart-to-icp}

Add the {{site.data.keyword.dcp_short}} Helm chart to the {{site.data.keyword.icpfull_notm}} internal repository.

1.  Ensure that you have enough disk space to load the images in the compressed files to your computer.

    You need 15 GB of space on your local system to support the extraction and loading of the archive file.

    1.  Check the Docker disk usage. Run the following command:

        ```bash
        docker system df
        ```

        For more command options, see [docker system df in the Docker documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/reference/commandline/system_df/).

    1.  If you need more disk space, take one of the following actions:

        - Remove old Docker images.
        - Increase the amount of storage that the Docker daemon uses. To increase the amount of storage that the Docker daemon uses, see the entry for `dm.basesize` in the [dockerd Docker documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/engine/reference/commandline/dockerd).

1.  If you have not, log in to your cluster from the IBM Cloud Private CLI and log in to the Docker private image registry.

    ```bash
    cloudctl login -a https://{cluster_CA_domain}/{deployment_name}:8443 --skip-ssl-validation
    ```
    ```bash
    docker login {icp-url}:8500
    ```

    Where `{icp-url}` is the certificate authority (CA) domain. If you did not specify a CA domain, the default value is `mycluster.icp`. See [Specifying your own certificate authority (CA) for IBM Cloud Private services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/installing/create_ca_cert.html).

## Step 4: Install the service
{: #install-wade-from-catalog}

- [4.1 Create persistent volumes](#create-pvs)
- [4.2 Install the service from the catalog](#admin-install)

### 4.1 Create persistent volumes
{: #create-pvs}

A PersistentVolume (PV) is a unit of storage in the cluster. In the same way that a node is a cluster resource, a persistent volume is also a resource in the cluster.

For an overview, see [Persistent Volumes in the Kubernetes documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/storage/persistent-volumes/).

When you install the service, persistent volume claims are created for the components automatically. However, because the preferred storage class for the service is local-storage, you must explicitly create persistent volumes before you install the service. Create 11 persistent volumes, one to accommodate each replica specified in the [system requirements](#sys-reqs) table earlier. See [Creating a PersistentVolume ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/manage_cluster/create_volume.html) for the steps to take to create one.

You must be a cluster administrator to create local storage volumes.
{: note}

### 4.2 Install the service from the catalog
{: #admin-install}

1.  From the Kubernetes command line tool, create the `discovery` namespace by using the following command:

    ```bash
    kubectl create namespace discovery
    ```
    {:codeblock}

    If you do not have the Kubernetes command line tool set up, complete the steps in [Prepare the cloud environment](#install-icp).

1.  Get a certificate from your {{site.data.keyword.icpfull_notm}} cluster and install it to Docker or add the {cluster_CA_domain}/{deployment_name} as a Docker Daemon insecure registry. You must do one or the other for Docker to be able to pull from your {{site.data.keyword.icpfull_notm}} cluster.

    See [Specifying your own certificate authority (CA) for IBM Cloud Private services ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/installing/create_ca_cert.html)

1.  To load the file from Passport Advantage into {{site.data.keyword.icpfull_notm}}, enter the following command in the {{site.data.keyword.icpfull_notm}} command line interface.

    ```bash
    cloudctl catalog load-archive --archive {archive_file_name} --registry {icp-url}:8500
    ```
    {: codeblock}

    - `{archive_file_name}` is the name of the file that you downloaded from Passport Advantage.
    - `{icp-url}` is the {{site.data.keyword.icpfull_notm}} cluster domain.

1.  View the charts in the {{site.data.keyword.icpfull_notm}} Catalog. From the {{site.data.keyword.icpfull_notm}} management console navigation menu, click **Manage** > **Helm Repositories**.
1.  Click **Sync Repositories**.

    You must have the *cluster administrator* user type or access level to sync repositories.

1.  From the navigation menu, select **Catalog**.
1.  Scroll to find the **ibm-watson-discovery-prod** package, and then click **Configure**.
1.  Specify values for the configurable fields.

    When you install the service, many configuration settings are applied to it for you unless you override them with your own values. You might want to change items such as user names and passwords for databases or stores that are created for you, for example. **You cannot change these settings after you complete the installation.**

    See [Configuration details](#config-details) for help understanding the configuration choices. At a minimum, you must provide your own value for the ICP cluster URL setting.

1.  Click **License agreements**.

    Click **Next** multiple times to read the full agreement, and then click **Accept** to accept the license terms.

1.  Click **Install**.

You might get a message that a timeout occurred during the installation process. However, the message can be ignored; the installation continues in the background. Give it time to complete. Check the Helm releases page for the status. See [Verify that the installation was successful](#verify).
{: important}

#### Configuration details
{: #config-details}

| Setting | Description |
|---------|-------------|
| ICP Cluster URL | Specify the cluster_CA_domain hostname of your private cloud instance. For example: `my.company.name.icp.net`. Specify the hostname only, without a protocol prefix (`https://`) and without a port number (`:8443`). This unique URL is typically referred to as `{icp-url}` in this documentation. |
{: caption="Configuration settings" caption-side="top"}

### Uninstalling the service

If you need to start the deployment over, be sure to remove all trace of the current installation before you try to install again.

Per the RBAC requirements, if the administrator has given the `default` service account the necessary permissions, no manual cleanup is needed. If this is not the case, you need to delete all resources with `{release_name}` in the name.
{: note}

1.  To uninstall and delete the `my-release` deployment, run the following command from the Helm command line interface:

    ```bash
    helm delete --tls my-release
    ```
    {: codeblock}

    To irrevocably uninstall and delete the `my-release` deployment, run the following command:

    ```bash
    helm delete --purge --tls my-release
    ```
    {: codeblock}

    If you omit the `--purge` option, Helm deletes all resources for the deployment but retains the record with the release name. This allows you to roll back the deletion. If you include the `--purge` option, Helm removes all records for the deployment so that the name can be used for another installation.

1.  Remove all content from any persistent volumes that you used for the previous deployment before you restart the installation. See [Deleting a PersistentVolume ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/manage_cluster/delete_volume.html) for more information.

## Step 5: Verify that the installation was successful
{: #verify}

To check the status of the installation process:

1.  Log in to the {{site.data.keyword.icpfull_notm}} management console.
1.  From the main menu, expand **Workloads**, and then choose **Helm releases**.
1.  Find the release name you used for the deployment in the list.

To run a test Helm chart:

1.  From the Helm command line interface, run the following command:

    ```bash
    helm test --tls {release-name} --timeout 900
    ```

1.  If one of the tests fails, review the logs to learn more. To see the log, use a command with the syntax `kubectl logs {testname} -n {namespace} -f --timestamps`. For example:

    ```bash
    kubectl logs my-release-bdd-test -n {namespace} -f --timestamps
    ```

1.  To run the test script again, first delete the test pods by using a command with the syntax `kubectl delete pod {podname} --namespace {namespace}`. For example:

    ```bash
    kubectl delete pod my-release-bdd-test --namespace default
    ```

### Installing from the command line
{: #cli}


If you do not want to install from the catalog, you can install from the command line. To do so, complete these steps:

1.  If you have not completed Steps 1 and 2 from the [Install the service from the catalog](#admin-install) procedure, do so now to create the namespace and install your cluster certificate in the Docker registry.

1.  After you specify your Docker image registry details, you can install the chart from the Helm command line interface. Enter the following command from the directory where the package was loaded in your local system:

    ```bash
    helm install --name {my_release} ibm-watson-discovery-prod --set global.icpDockerRepo="{cluster_CA_domain}/{deployment_name}:8500/{namespace} --set license=accept"
    ```
    {: codeblock}

    - Replace `{my_release}` with a name for your release.
    - Replace `{cluster_CA_domain}/{deployment_name}` with your ICP domain.
    - Replace `{namespace}` with the release's namespace.
    - The `ibm-watson-discovery-prod` parameter represents the name of the Helm chart.

## Step 6: Launch the Watson Assistant Discovery Extension tooling
{: #launch-tool}

1.  Log in to the {{site.data.keyword.icpfull_notm}} management console.
1.  From the main menu, expand **Workloads**, and then choose **Deployments**.
1.  Find the deployment named `{release-name}-tooling`.

    If you don't know the `{release-name}`, ask your administrator.

1.  Click **Launch**.

    A new web browser tab opens and shows the {{site.data.keyword.dcp_short}} tool login page. For example: `https://discovery.{icp-url}`.
1.  Log in using the same credentials you used to log into the {{site.data.keyword.icpfull_notm}} dashboard.

## Next steps
{: #next-steps-install}

Use the {{site.data.keyword.dcp_short}} tool to 

- To learn more about the service first, read the [overview](/docs/services/discovery-icp/index.html#wadex-about).
- To see how it works for yourself, follow the steps in [Getting started](/docs/services/discovery-icp/getting-started.html#gs-api).



