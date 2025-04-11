---

copyright:
  years: 2025
lastupdated: "2025-04-11"

keywords:

subcollection: pattern-db2-sap-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deploying highly available SAP with Db2
{: #deploy-db2-sap}

The highly available SAP with Db2 deployment process requires specific deployment and configuration steps.

1. Deploy {{site.data.keyword.cloud_notm}} infrastructure resources
1. Install the required software
1. Configure Db2 Pacemaker
1. Deploy SAP
1. Test the environment

## Deploy {{site.data.keyword.cloud_notm}} infrastructure resources
{: #deploy-cloud-infra}

The first step is to deploy the infrastructure, Virtual System Instances (VSIs), or Bare Metal servers to support the {{site.data.keyword.IBM_notm}} Db2 and SAP Pacemaker clusters.

### Installing Virtual Server infrastructure
{: #deploy-cloud-infra-vsi}

If you are using Virtual System Instances (VSIs), you must complete the following steps:

* Setting up a Virtual Private Cloud (VPC) and subnet
* Provisioning Intel virtual servers
* Adding Block Storage for VPC
* Creating the network interfaces
* Readying your Operating System

For more information about these steps, see [Deploying your VPC VSI infrastructure](/docs/sap?topic=sap-vs-set-up-infrastructure).

### Installing Bare Metal Server infrastructure
{: #deploy-cloud-infra-bm}

If you are using Bare Metal servers, you must complete the following steps:

1. Setting up a Virtual Private Cloud (VPC) and subnet
1. Provisioning Intel bare metal servers
1. Creating the network interfaces
1. Readying your Operating System
1. Defining the storage layout for your Bare Metal servers

For more information about these steps, see [Deploying your Bare Metal infrastructure](/docs/sap?topic=sap-bm-vpc-set-up-infrastructure).

## Install the required software
{: #deploy-reqd-sw}

After the infrastructure has been deployed, install the software that is required to support the IBM Db2 and SAP Pacemaker clusters.

* Installing SAP and IBM Db2 software
* Installing the Pacemaker software in the SAP cluster
* Installing the Pacemaker software in the IBM Db2 cluster

### Installing the SAP and IBM Db2 software
{: #deploy-db2-pm}

{{site.data.keyword.cloud_notm}} provides a fully managed Db2 service by using the [catalog](/catalog/services/db2){: external}. The pattern doesn't use this cloud service and requires Db2 to be manually installed. {: important}

### Download and install SAP software and applications
{: #deploy-sap-sw}

SAP software installation media must be obtained from SAP directly, and requires valid license agreements with SAP in order to access these files.

* [Download and install SAP software and applications](/docs/sap?topic=sap-download-install-media)

### Installing the Pacemaker software in the SAP cluster
{: #deploy-sap-pm-install}

SAP uses the Pacemaker software that is provided by the Linux distribution that you are using. Follow the appropriate steps for the Linux distribution that's being used: 

* [Installing Pacemaker on Red Hat Enterprise Linux](https://www.redhat.com/en/blog/rhel-pacemaker-cluster){: external}
* [Installing Pacemaker on SUSE Linux Enterprise Server](https://documentation.suse.com/sle-ha/12-SP5/html/SLE-HA-all/art-ha-install-quick.html){: external}

### Installing the Pacemaker software in the IBM Db2 cluster
{: #deploy-db2-pm-install}

IBM Db2 provides its own Pacemaker cluster software package that is intended for use with Db2. This must be used instead of the Pacemaker software that is provided by the Linux distribution. The following links describe the installation of the IBM Db2 Pacemaker software:

* [Installing the Pacemaker cluster software stack](https://www.ibm.com/docs/en/db2/12.1?topic=manager-installing-pacemaker-cluster){: external}
* [Installing Pacemaker with IBM Db2 SAP Help Portal](https://help.sap.com/docs/DB6/e3eefec5d20740f4872652a475457348/f725ea789c4043e5bee3d0fafd07bc9e.html){: external}


## Configure Db2 Pacemaker
{: #deploy-db2-config}

The {{site.data.keyword.IBM_notm}} Db2 Pacemaker configuration requires two steps:

1. Setting up a Virtual IP address with an Application Load Balancer
1. Configuring Db2 HADR Pacemaker cluster fencing or Qdevice on {{site.data.keyword.cloud_notm}}

### Virtual IP address setup
{: #deploy-db2-vip}

A Virtual IP address (VIP) is used for communication between the Db2 database and the application. This is routed by the {{site.data.keyword.cloud_notm}} Application Load Balancer (ALB). The Application Load Balancer service with failover support routes the client traffic to a given database in a Db2 HADR cluster.

{{site.data.keyword.cloud_notm}} Application Load Balancers use front end listeners, security groups, backend pools, and health checks to route the traffic. For more information, see [About application load balancers](/docs/vpc?topic=vpc-load-balancers-about).

For more information on the steps to configure this Virtual IP address, see [Setting up Virtual IP address for two-node Db2 HADR Pacemaker cluster with Application Load Balancer on {{site.data.keyword.cloud_notm}}](https://www.ibm.com/support/pages/node/7184308){:external}.

### Pacemaker fencing agent setup
{: #deploy-db2-fence}

Pacemaker provides different mechanisms to handle node failures and in particular the 'split brain' scenario in a two-node cluster. One mechanism is to use a third **quorum node** within the Pacemaker cluster to arbitrate when one of the HADR cluster nodes fails.

On {{site.data.keyword.cloud_notm}}, while you can configure a quorum device on a third host, there is an alternative that uses a **fencing agent**. The fencing agent interacts with the {{site.data.keyword.cloud_notm}} infrastructure to start, stop, monitor and/or reboot the virtual machines. The advantage of configuring a two-node HADR Pacemaker cluster with fencing is that it removes the requirement of a third host for the quorum device, thus reducing ongoing cost. 

For more information on configuring Pacemaker with the *fence_ibm_vpc* fencing agent, see [Setting up two-node Db2 HADR Pacemaker cluster with fencing on {{site.data.keyword.cloud_notm}}](https://www.ibm.com/support/pages/node/7228864){: external}

## Deploy SAP
{: #deploy-sap}

{{site.data.keyword.IBM_notm}} provides several templates with scripts to deploy different SAP NetWeaver and IBM Db2 architectures.  These use Terraform or Terraform and Ansible to deploy SAP. For more information, see [SAP Terraform deployment templates](/docs/sap?topic=sap-terraform-templates).

## Testing
{: #deploy-testing}

The final step is to test the environment to ensure that failures are handled as expected by the Pacemaker cluster. The tests depend on the configuration that has been implemented and might include:

1. Manually moving the SAP ASCS instance
1. Manually moving the SAP ERS instance
1. Testing failure of the SAP ASCS instance
1. Testing failure of the SAP ERS instance
1. Manually moving the Db2 Primary database instance
1. Manually moving the Db2 Secondary database instance
1. Testing failure of the Db2 primary database instance
1. Testing failure of the Db2 Secondary database instance
1. Failure of SAP ASCS instance due to node crash
1. Failure of SAP ERS instance due to node crash
1. Failure of Db2 primary database instance due to node crash
1. Failure of Db2 Secondary database instance due to node crash
