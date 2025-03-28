---

copyright:
  years: 2025
lastupdated: "2025-03-28"

keywords:

subcollection: pattern-db2-sap-vpc

---

{{site.data.keyword.attribute-definition-list}}

# Deploying Highly Available SAP with Db2 on IBM Cloud
{: #deploy-db2-sap}

This document provides an overview of the process through which the Highly Available SAP with Db2 on IBM Cloud pattern can be deployed. Links are provided to other sources of information that describe the specific deployment and configuration steps that are required.

The deployment process consists of the following steps:

1. Deploy IBM Cloud infrastructure resources
1. Install the required software
1. Configure Db2 Pacemaker
1. Deploy SAP
1. Testing

## Deploy IBM Cloud infrastructure resources
{: #deploy-cloud-infra}

The first step is to deploy the infrastructure - Virtual System Instances (VSIs) or Bare Metal servers - that will be used to support the IBM Db2 and SAP Pacemaker clusters.

### Installing Virtual Server infrastructure
{: #deploy-cloud-infra-vsi}

If you are using Virtual System Instances (VSIs), the following steps will need to be followed:

1. Setting up a Virtual Private Cloud (VPC) and subnet
1. Provisioning Intel virtual servers
1. Adding Block Storage for VPC
1. Creating the network interfaces
1. Readying your Operating System

For more information and a walkthrough of these steps, see [Deploying your VPC VSI infrastructure](/docs/sap?topic=sap-vs-set-up-infrastructure).

### Installing Bare Metal Server infrastructure
{: #deploy-cloud-infra-bm}

If you are using Bare Metal servers, the following steps will need to be followed:

1. Setting up a Virtual Private Cloud (VPC) and subnet
1. Provisioning Intel bare metal servers
1. Creating the network interfaces
1. Readying your Operating System
1. Defining the storage layout for your Bare Metal servers

For more information and a walkthrough of these steps, see [Deploying your Bare Metal infrastructure](/docs/sap?topic=sap-bm-vpc-set-up-infrastructure).

## Install the required software
{: #deploy-reqd-sw}

After the infrastructure has been deployed, the next step installs the software that is required to support the IBM Db2 and SAP Pacemaker clusters.

1. Installing IBM Db2 software
1. Installing SAP software
1. Installing the Pacemaker software in the SAP cluster
1. Installing the Pacemaker software in the IBM Db2 cluster

### Installing the IBM Db2 software
{: #deploy-db2-pm}

IBM Cloud provides a fully managed Db2 service via the IBM Cloud catalog (https://cloud.ibm.com/catalog/services/db2). This pattern **does not** use this cloud service and requires Db2 to be manually installed. {: important}

The following link describes the installation of the IBM Db2 software:

* [Installing Db2 Database Servers](https://www.ibm.com/docs/en/db2/12.1?topic=installing-db2-database-servers)

### Download and install SAP software and applications
{: #deploy-sap-sw}

SAP software installation media must be obtained from SAP directly, and requires valid license agreements with SAP in order to access these files. The following link describes this process:

* [Download and install SAP software and applications](/docs/sap?topic=sap-download-install-media)

### Installing the Pacemaker software in the SAP cluster
{: #deploy-sap-pm-install}

SAP uses the Pacemaker software provided by the Linux distribution that you are using. Follow the appropriate steps for the Linux distribution being used

* [Installing Pacemaker on Red Hat Enterprise Linux](https://www.redhat.com/en/blog/rhel-pacemaker-cluster)
* [Installing Pacemaker on SUSE Linux Enterprise Server](https://documentation.suse.com/sle-ha/12-SP5/html/SLE-HA-all/art-ha-install-quick.html)

### Installing the Pacemaker software in the IBM Db2 cluster
{: #deploy-db2-pm-install}

IBM Db2 provides its own Pacemaker cluster software package that is intended for use with Db2. This should be used instead of the Pacemaker software provided by the Linux distribution. The following link describes the installation of the IBM Db2 Pacemaker software:

* [Installing the Pacemaker cluster software stack](https://www.ibm.com/docs/en/db2/12.1?topic=manager-installing-pacemaker-cluster)

## Configure Db2 Pacemaker
{: #deploy-db2-config}

The IBM Db2 Pacemaker configuration requires two steps:

1. Setting up a Virtual IP address with an Application Load Balancer
1. Configuring Db2 HADR Pacemaker cluster fencing on IBM Cloud

Links to the configuration activities for each step are provided in the next sections.

### Virtual IP Address setup
{: #deploy-db2-vip}

A Virtual IP address (VIP) is used for communication between the Db2 database and the application. This is routed by the IBM Cloud Application Load Balancer (ALB). The Application Load Balancer service with failover support routes the client traffic to a given database in a Db2 HADR cluster.

IBM Cloud Application Load Balancers use frontend listeners, security groups, backend pools and health checks to route the traffic. To get more information on the Application Load Balancer, refer to: https://cloud.ibm.com/docs/vpc?topic=vpc-load-balancers-about

For more information on the steps to configure this Virtual IP address, see [Setting up Virtual IP address for two-node Db2 HADR Pacemaker cluster with Application Load Balancer on IBM Cloud](https://www.ibm.com/support/pages/node/7184308).

### Pacemaker fencing agent setup
{: #deploy-db2-fence}

Pacemaker provides different mechanisms to handle node failures and in particular the 'split brain' scenario in a two node cluster. One mechanisim is to use a third **quorum node** within the Pacemaker cluster to arbitrate when one of the HADR cluster nodes fails.

On IBM Cloud, while you can configure a quorum device on a third host, there is an alternative that uses a **fencing agent**. The fencing agent interacts with the IBM Cloud infrastructure to start, stop,  or reboot the virtual machines. The advantage of configuring a two-node HADR Pacemaker cluster with fencing is that it removes the requirement of a third host for the quorum device, thus reducing on-going cost. 

For more information on configuring Pacemaker with the *fence_ibm_vpc* fencing agent, see [Setting up two-node Db2 HADR Pacemaker cluster with fencing on IBM Cloud](https://www.ibm.com/support/pages/node/7228864).

## Deploy SAP
{: #deploy-sap}

IBM provides several templates with scripts to deploy different SAP NetWeaver and SAP HANA architectures.  These use Terraform or Terraform and Ansible to deploy SAP. For more information, see [SAP Terraform deployment templates](/docs/sap?topic=sap-terraform-templates).

## Testing
{: #deploy-testing}

The final step is to test the environment to ensure that failures are handled as expected by the Pacemaker cluster.  The tests to be performed will depend upon the configuration that has been implemented, but might include:

1. Manually moving the SAP ASCS instance
1. Manually moving the SAP ERS instance
1. Testing failure of the SAP ASCS instance
1. Testing failure of the SAP ERS instance
1. Manually moving the Db2 Primary database instance
1. Manually moving the Db2 Secondary database instance
1. Testing failure of the Db2 Primary database instance
1. Testing failure of the Db2 Secondary database instance
1. Failure of SAP ASCS instance due to node crash
1. Failure of SAP ERS instance due to node crash
1. Failure of Db2 Primary database instance due to node crash
1. Failure of Db2 Secondary database instance due to node crash
