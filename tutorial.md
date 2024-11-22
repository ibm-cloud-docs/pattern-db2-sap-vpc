---

copyright:
   years: 2020
lastupdated: "2024-11-22"

keywords:

subcollection: pattern-db2-sap-vpc

content-type: tutorial
services: containers, Registry, <subcollection names from toc> # Only if the tutorial includes multiple services. If it only uses your service, don't specify. DO NOT set any platform subcollections.
account-plan: lite # Set `lite` if tutorial can be completed by using only Lite plan services; Set `paid` if the tutorial requires a pay-go or subscription versions of plans for the service
completion-time: 10m # Estimated time to complete the steps in this tutorial. Minute values are supported up to 90 minutes. Whole hours are also supported; for example: 2h

---

{{site.data.keyword.attribute-definition-list}}




# Set up continuous deployment to Kubernetes
{: #tutorial-cd-kube}
{: toc-content-type="tutorial"} 
{: toc-services="containers, Registry"} 
{: toc-completion-time="10m"} 



In this tutorial, you learn how to set up a continuous integration and delivery pipeline for containerized applications running on the {{site.data.keyword.containershort_full}}. You set up source control, and then build, test, and deploy the code to different deployment stages. Then, you add integrations to other services like Slack notifications.
{: shortdesc}



![Architectural diagram](images/image.svg)
{: figure caption="Figure 1. A diagram that shows the architecture for my tutorial."}

The pipeline that you create has the following architecture:
1. Workflow step 1
1. Workflow step 2
1. Workflow step 3
1. Workflow step 4

## Before you begin
{: #cd-kube-prereqs}





* [Install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started).
* [Set up the {{site.data.keyword.cloud_notm}} Container Registry CLI and your registry namespace](/docs/services/Registry?topic=registry-registry_setup_cli_namespace).
* [Understand the basics of Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/).



## Create a development Kubernetes cluster
{: #cd-kube-dev-cluster}
{: step}



First, you need to set up a Kubernetes cluster on the {{site.data.keyword.containershort_notm}} service. {{site.data.keyword.containershort_notm}} delivers powerful tools by combining Docker and Kubernetes technologies, an intuitive user experience, and built-in security and isolation to automate the deployment, operation, scaling, and monitoring of containerized apps in a cluster of compute hosts.

1. In the IBM Cloud catalog, go to the [Kubernetes Service](/kubernetes/catalog/cluster/create).
1. Select **Standard** as the cluster type, and select **2 MB / 1 Worker** as the machine type. All other options can be left as default.
1. Click **Create** to create your cluster. Check the status of your cluster and worker nodes until they're in the Ready state.

You'll need to wait until your workers are ready to move to the next step.
{: note}

## Build your app locally
{: #cd-kube-build-app}
{: step}



You can build and run the application as you normally would using `mvn` for Java&trade; local development or `npm` for Node.js development.  You can also build a Docker image and run the application in a container to ensure consistent execution locally and on the cloud. Use the following steps to build your docker image.

1. Ensure your local Docker engine is started.

   ```sh
   docker ps
   ```
   {: pre}

1. Navigate to the generated project directory.

   ```sh
   cd <project name>
   ```
   {: pre}

1. Build the application locally.

   ```sh
   ibmcloud dev build
   ```
   {: pre}

   This might take a few minutes to run because all of the application dependencies are downloaded and a Docker image, which contains your application and all the required environment, is built.

## Add a task-oriented title
{: #cd-kube-step-desc}
{: step}

## Add a task-oriented title
{: #cd-kube-step-desc}
{: step}

## Add a task-oriented title
{: #cd-kube-step-desc}
{: step}

## Next steps
{: #cd-kube-step-next}

Want to start fresh? Remove the following resources that you created as a part of this tutorial:

* Delete the Git repository.
* Delete the toolchain.
* Delete the cluster.
* Delete the Slack channel.
