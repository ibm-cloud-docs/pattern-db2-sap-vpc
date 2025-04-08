---

copyright:
  years: 2025
lastupdated: "2025-04-08"

keywords: SAP, Db2, Pacemaker, SUSE, SLES, SUSE Linux, High Availability, cluster

subcollection: pattern-db2-sap-vpc 

authors:
  - name: John Easton
    url: https://linkedin.com/in/johnpeaston
  - name: "John Easton"
    url: "linkedIn profile URL"

version: 1.0

deployment-url: url

# docs: https://cloud.ibm.com/docs/solution-guide

# image_source: https://github.com/terraform-ibm-modules/module/reference-architectures/xxx.svg

# related_links:
#  - title: 'Highly Available SAP with Db2 on IBM Cloud VPC'
#    url: 'https://url.com'
#    description: 'Deploying highly available SAP and Db2 on IBM Cloud VPC using SUSE Linux.'
#  - title: 'Related or follow-on architectures'
#    url: 'https://cloud.ibm.com/docs/pattern-sap-on-vpc'
#    description: 'An IBM® solution design for the deployment of SAP on IBM Cloud® Virtual Private Cloud (VPC).'

use-case: Data resiliency, Enterprise resource planning
industry: FinancialSector, Manufacturing, Retail, Industrials
compliance: ISOIEC27001
content-type: reference-architecture

production: false

---

{{site.data.keyword.attribute-definition-list}}

# Highly Available SAP with Db2 on IBM Cloud VPC
{: #pattern-db2-sap-vpc}
{: toc-content-type="reference-architecture"}
{: toc-industry="FinancialSector, Manufacturing, Retail, Industrials"}
{: toc-use-case="Data resiliency, Enterprise resource planning"}
{: toc-compliance="ISOIEC27001"}
{: toc-version="1.0"}

Many organisations run SAP applications using an IBM Db2 database to support the SAP instance. This pattern describes a highly available implementation of both SAP and Db2 to deliver a resilient solution to meet an organisation's business needs.

SAP systems are often mission-critical for the organisations that use them. Simply providing a highly-available SAP Application Server will offer minimal benefit unless the underlying database - IBM Db2 in this case - is also made highly available. The architecture for this pattern provides a highly available database layer which underpings a highly available application server layer running within {{site.data.keyword.Bluemix}} Virtual Private Cloud.

## Architecture diagram
{: #architecture-diagram}

The following diagram shows the reference architecture for this pattern:

![Architecture diagram for the Highly Available SAP with Db2 on IBM Cloud VPC pattern](/images/sap-db2-vpc-detailedHLA.drawio.svg "Architecture diagram for the Highly Available SAP with Db2 on IBM Cloud VPC pattern"){: caption="Architecture diagram for the Highly Available SAP with Db2 on IBM Cloud VPC pattern" caption-side="bottom"}

Users will access the {{site.data.keyword.Bluemix}} environment either via a Virtual Private Network (VP) across the Internet, or via a private Direct Link connection.

Administrative access to the environment is recommended to go via a Bastion Host. This provides a secure access point for systems management activities. Bastion hosts can also provide session logging for audit purposes, allowing all management activities to be securely logged. In the event of an incident or problem arising, there is a record of what actions were performed that gave rise to this.

As was briefly described, the underlying IBM Db2 database is made highly available by using multiple database server nodes. In the event of a failure of the primary database server, there is a secondary (backup) server available to take over the primary's workload. The cluster of nodes is managed by the IBM Db2 integrated Pacemaker cluster management software (Pacemaker). This monitors the health of the environment and responds to incidents by managing the failover of the database from one node to another.

The SAP Application server instances run in a separate Pacemaker-managed cluster. As with the database cluster, Pacemaker manages the failover of the SAP components from one node to another following a failure.

## Design concepts
{: #design-concepts}

The following diagram shows the infrastructure and software stack for the solution:

![Highly Available SAP with Db2 on IBM Cloud VPC stack diagram.](images/sap+db2-stack.drawio.svg "Highly Available SAP with Db2 on IBM Cloud VPC stack diagram"){: caption="Highly Available SAP with Db2 on IBM Cloud VPC stack diagram" caption-side="bottom"}

This diagram shows how the various components of the pattern deliver high availability.

* IBM Db2 database data is replicated from the primary database server to the secondary (backup) database server using an IBM Db2 component called High Availability/Disaster Recovery (HADR).

* Users and applications (including SAP) access the databases via the IP address of the database service.  This is deployed as a Virtual IP (VIP) address.  Pacemaker will manage the failover of this VIP between the database servers when a failover occurs.

* There are two key SAP components in the SAP instance, ABAP SAP Central Services (ASCS) and Enqueue Replication Server (ERS). Just as Db2 uses it's HADR component to replicate data between the servers in its Pacemaker cluster, SAP uses a technique called Enqueue Replication to do the same.

* SAP applications also access the SAP Application Server and ASCS via a Virtual IP address that can move when failures occur. Again, Pacemaker manages the movement of the VIPs for both ASCS and ERS as required.

Review the design considerations and architecture decisions for the following aspects and domains:

- **Compute:** Virtual Servers or Bare Metal Servers
- **Storage:** Primary Storage
- **Networking:** Enterprise Connectivity, Load Balancing and Domain Name Services
- **Application Platforms:** Enterprise Applications
- **Resiliency:** High Availability
- **Service Management:** Management and Orchestration

![Architecture Design Scope](images/ha-db2-sap-heat-map.drawio.svg "Architecture Design Scope"){: caption="Architecture Design Scope" caption-side="bottom"}

### Design choices
{: #design-choices}

#### Cluster management software
{: #cluster-mgmt-sw}

There are a number of different solutions available that provide cluster management, both open source and proprietary. This pattern uses the open source Pacemaker software to manage the cluster resources for the SAP Applications and the IBM Db2 integrated Pacemaker to manage the Db2 resources, controlling and orchestrating the failover and failback of resources when the cluster configuration changes as cluster nodes leave and rejoin the cluster.

Other solutions from the open source community or from other commercial organizations are also available. Refer to the websites of the open source projects or the documentation provided by commercial organizations for further information on how these could be used within this architecture.

#### Storage options
{: #storage-options}

The pattern uses two key storage options. IBM Cloud provides VPC File Storage.  This can be used to provide a shared filesystem that is accessed by the SAP Application Server nodes within the Pacemaker cluster. 

Block storage for the database instances (both primary and secondary) is delivered through VPC Block Storage if VSIs are used as database servers. If the decision is made to use VPC Bare Metal servers, this storage can be provided by solid state drives within the physical server.  This offers faster performance than VPC Block Storage that is accessed via the network.

#### Application Server nodes
{: #appsvr-nodes}

Two options are available for Application Server nodes, Virtual Servers (VSIs) or Bare Metal servers. There are many different VSI profiles that can be chosen to best meet the compute and memory requirements of the application(s) being run within the environment but these need to be supported by SAP. 

#### Database Server nodes
{: #dbsvr-nodes}

Two options are available for Database Server nodes, Virtual Servers (VSIs) or Bare Metal servers. There are many different VSI profiles that can be chosen to best meet the compute and memory requirements of the database.

## Requirements
{: #requirements}

The following table outlines the requirements that are addressed in this architecture.

| Aspect | Requirements |
| -------------- | -------------- |
| Compute            | Provide properly isolated compute resources with adequate compute capacity for the applications. |
| Storage            | Provide storage that meets the application server and database performance requirements. |
| Networking         | Deploy workloads in isolated environment and enforce information flow policies. \n Provide secure, encrypted connectivity to the cloud’s private network for management purposes. \n Distribute incoming application requests across available compute resources. \n Support failover of application and database to alternate servers in the event of planned or unplanned outages \n Provide private DNS resolution to support use of hostnames instead of IP addresses and support cluster failover. |
| Security           | Ensure all operator actions are executed securely through a bastion host. \n Protect the boundaries of the application against denial-of-service and application-layer attacks. \n Encrypt all application data in transit and at rest to protect from unauthorized disclosure. \n Encrypt all backup data to protect from unauthorized disclosure. \n Encrypt all security data (operational and audit logs) to protect from unauthorized disclosure. \n Encrypt all data using customer managed keys to meet regulatory compliance requirements for additional security and customer control. \n Protect secrets through their entire lifecycle and secure them using access control measures. |
| Resiliency         | Support application availability targets and business continuity policies. \n Ensure availability of the application in the event of planned and unplanned outages. \n Provide highly available compute, storage, network, and other cloud services to handle application load and performance requirements. \n Backup application data to enable recovery in the event of unplanned outages. \n Provide highly available storage for security data (logs) and backup data. \n Automate recovery tasks to minimize down time |
| Service Management | Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. \n Generate alerts/notifications about issues that might impact the availability of applications to trigger appropriate responses to minimize down time. \n Monitor audit logs to track changes and detect potential security problems. \n Provide a mechanism to identify and send notifications about issues found in audit logs. |
{: caption="Table 1. Requirements" caption-side="bottom"}

## Components
{: #components}

The following table outlines the products or services used in the architecture for each aspect:

| Aspects | Architecture component | How the component is used |
| -------------- | -------------- | -------------- |
| Compute | [Virtual Servers for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers&interface=ui) | Application Server and Database Server nodes |
|  | [Bare Metal Servers for VPC](/docs/vpc?topic=vpc-about-bare-metal-servers) | Application Server and Database Server nodes (option) |
| Storage | [VPC File Storage](/docs/vpc?topic=vpc-file-storage-vpc-about) | File storage used by SAP Application Servers |
|  | [VPC Block Storage](/docs/vpc?topic=vpc-block-storage-about) | File storage used by Db2 Database Servers |
| Networking | [Virtual Private Endpoint (VPE)](/docs/vpc?topic=vpc-about-vpe) | For private network access to Cloud Services, e.g., Key Protect, IAM, etc. |
|  | [Public Gateway](/docs/vpc?topic=vpc-about-public-gateways) | For secure client access to the HPC environment over the Internet |
|  | [Direct Link](/docs/dl?topic=dl-dl-about) | For private, dedicated connectivity between on-premises and cloud HPC resources |
| Security | [Identity and Access Management](https://cloud.ibm.com/iam/overview){: external} | Identity and Access Management |
|  | [Key protect](/docs/key-protect?topic=key-protect-about) or [Hyper Protect Crypto Services](/docs/hs-crypto?topic=hs-crypto-get-started) | Hardware security module (HSM) and Key Management Service |
|  | [Secrets Manager](/docs/secrets-manager?topic=secrets-manager-getting-started) | Certificate and Secrets Management |
| Resiliency | [Pacemaker](/https://clusterlabs.org/projects/pacemaker) | The Pacemaker software manages failures of application or database server nodes by managing failover to a surviving cluster node (VSI or Bare Metal) |
| Service Management | [IBM Cloud Monitoring](/docs/monitoring?topic=monitoring-about-monitor) | Operational monitoring |
|  | [IBM Cloud](/docs/cloud-logs) | For all IBM Cloud related Logs |
|  | [Event Routing](/docs/cloud-logs) | For all IBM Cloud events  |
{: caption="Components" caption-side="bottom"}
