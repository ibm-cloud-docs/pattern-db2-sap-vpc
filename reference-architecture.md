---

copyright:
  years: 2025
lastupdated: "2025-04-29"

keywords: SAP, Db2, Pacemaker, SUSE, SLES, SUSE Linux, High Availability, cluster

subcollection: pattern-db2-sap-vpc 

authors:
  - name: John Easton
    url: https://linkedin.com/in/johnpeaston

version: 1.0

deployment-url: url

use-case: Data resiliency, Enterprise resource planning
industry: FinancialSector, Manufacturing, Retail, Industrials
compliance: ISOIEC27001
content-type: reference-architecture

production: false

---

{{site.data.keyword.attribute-definition-list}}

# Highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC
{: #pattern-db2-sap-vpc}
{: toc-content-type="reference-architecture"}
{: toc-industry="FinancialSector, Manufacturing, Retail, Industrials"}
{: toc-use-case="Data resiliency, Enterprise resource planning"}
{: toc-compliance="ISOIEC27001"}
{: toc-version="1.0"}

Many organizations run SAP applications by using an {{site.data.keyword.Bluemix_notm}} Db2 database to support the SAP instance. This pattern describes a highly available implementation of both SAP and Db2 to deliver a resilient solution to meet an organization's business needs.

SAP systems are often mission-critical for the organizations that use them. Providing a highly available SAP Application Server offers minimal benefit unless the underlying database - {{site.data.keyword.IBM_notm}} Db2 in this case - is also made highly available. The architecture for this pattern provides a highly available database layer, which underpins a highly available application server layer running within {{site.data.keyword.Bluemix}} Virtual Private Cloud.

## Architecture diagram
{: #architecture-diagram}

The following diagram shows the high level reference architecture for this pattern:

![High level architecture diagram for the highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC pattern](/images/sap-db2-vpc-detailedHLA.drawio.svg "High level architecture diagram for the highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC pattern"){: caption="High level architecture diagram for the highly available SAP with Db2 on  Cloud VPC pattern" caption-side="bottom"}

Users access the {{site.data.keyword.Bluemix}} environment by using a Virtual Private Network (VP) across the Internet or a private Direct Link connection.

Administrative access to the environment is recommended by using a bastion host. This access provides a secure access point for systems management activities. Bastion hosts can also provide session logging for audit purposes, allowing all management activities to be securely logged. If an incident or problem occurs, there is a record of what actions were performed that caused the issue.

As was briefly described, the underlying {{site.data.keyword.IBM_notm}} Db2 database is made highly available by using multiple database server nodes. If there is a failure of the primary database server, a secondary backup server is available to take over the primary's workload. The cluster of nodes is managed by the {{site.data.keyword.IBM_notm}} Db2 integrated Pacemaker cluster management software. This monitors the health of the environment and responds to incidents by managing the failover of the database from one node to another.

The SAP application server instances run in a separate Pacemaker-managed cluster. Similar to the database cluster, Pacemaker manages the failover of the SAP components from one node to another following a failure.

## Design concepts
{: #design-concepts}

The following diagram shows the infrastructure and software stack for the solution:

![Highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC stack diagram.](images/sap+db2-stack.drawio.svg "Highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC stack diagram"){: caption="Highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC stack diagram" caption-side="bottom"}

This diagram shows how the various components of the pattern deliver high availability.

* {{site.data.keyword.IBM_notm}} Db2 database data is replicated from the primary database server to the secondary backup database server by using an {{site.data.keyword.IBM_notm}} Db2 component called high availability disaster recovery (HADR).

* Users and applications including SAP access the databases through the IP address of the database service. This is deployed as a virtual IP (VIP) address. Pacemaker manages the failover of this VIP between the database servers when a failover occurs.

* Two key SAP components in the SAP instance are available, the ABAP SAP Central Services (ASCS) and Enqueue Replication Server (ERS). Just as Db2 uses its HADR component to replicate data between the servers in its Pacemaker cluster, SAP uses a technique that is called Enqueue Replication to do the same.

* SAP applications also access the SAP application server and ASCS through a virtual IP address that can move between systems when failures occur. Pacemaker manages the movement of the virtual IPs for both ASCS and ERS as required.

Review the design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual servers or bare metal servers
- Storage: Primary storage
- Networking: Enterprise connectivity, load balancing, and Domain Name Services(DNS)
- Application platforms: Enterprise applications
- Resiliency: High availability
- Service management: Management and orchestration

![Architecture Design Scope](images/ha-db2-sap-heat-map.drawio.svg "Architecture Design Scope"){: caption="Architecture Design Scope" caption-side="bottom"}

### Design choices
{: #design-choices}

#### Cluster management software
{: #cluster-mgmt-sw}

Various different solutions are available that provide cluster management, both open source and proprietary. This pattern uses the open source Pacemaker software to manage the cluster resources for the SAP applications and the {{site.data.keyword.IBM_notm}} Db2 integrated Pacemaker to manage the Db2 resources, controlling and orchestrating the failover and failback of resources when the cluster configuration changes as cluster nodes leave and rejoin the cluster.

Other solutions from the open source community or from other commercial organizations are also available. Check out the open source projects websites or the documentation that is provided by commercial organizations for more information on how these can be incorporated within this architecture.

#### Storage options
{: #storage-options}

The pattern uses two key storage options. {{site.data.keyword.Bluemix_notm}} provides VPC File Storage. This can be used to provide a shared file system that is accessed by the SAP application server nodes within the Pacemaker cluster. 

Block storage for the database instances, both primary and secondary, is delivered through {{site.data.keyword.bm_is_full}} if VSIs are used as database servers. If the decision is made to use {{site.data.keyword.bm_is_short}}, this storage can be provided by solid-state drives within the physical server. This offers faster performance than VPC Block Storage that is accessed across the network.

#### Application server nodes
{: #appsvr-nodes}

Two options are available for application server nodes, virtual servers (VSIs) or {{site.data.keyword.baremetal_short}}. There are many different VSI profiles that can be chosen to best meet the compute and memory requirements of the application that are run within the environment. SAP must support these VSI profiles. 

#### Database server nodes
{: #dbsvr-nodes}

Two options are available for database server nodes, virtual servers (VSIs), or {{site.data.keyword.baremetal_short}}. There are many different VSI profiles that can be chosen to best meet the compute and memory requirements of the database.

## Requirements
{: #requirements}

The following table outlines the requirements that are addressed in this architecture.

| Aspect | Requirements |
| -------------- | -------------- |
| Compute            | Provide properly isolated compute resources with adequate compute capacity for the applications. |
| Storage            | Provide storage that meets the application server and database performance requirements. |
| Networking         | Deploy workloads in an isolated environment and enforce information flow policies. \n Provide secure, encrypted connectivity to the cloud’s private network for management purposes. \n Distribute incoming application requests across available compute resources. \n Support failover of the application and database to alternative servers if a planned or unplanned outage occurs. \n Provide private DNS resolution to support use of hostnames instead of IP addresses and support cluster failover. |
| Security           | Ensure that all operator actions are run securely through a bastion host. \n Protect the boundaries of the application against denial-of-service and application-layer attacks. \n Encrypt all application data in transit and at rest to protect it from unauthorized disclosure. \n Encrypt all backup data to protect it from unauthorized disclosure. \n Encrypt all security data (operational and audit logs) to protect from unauthorized disclosure. \n Encrypt all data by using customer-managed keys to meet regulatory compliance requirements for additional security and customer control. \n Protect secrets through their entire lifecycle and secure them using access control measures. |
| Resiliency         | Support application availability targets and business continuity policies. \n Ensure availability of the application if a planned and unplanned outage occurs. \n Provide highly available compute, storage, network, and other cloud services to handle application load and performance requirements. \n Backup application data to enable recovery if an unplanned outage occurs. \n Provide highly available storage for security data such as logs and backup data. \n Automate recovery tasks to minimize downtime. |
| Service management | Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. \n Generate alerts and notifications about issues that might impact the availability of applications to trigger appropriate responses to minimize downtime. \n Monitor audit logs to track changes and detect potential security problems. \n Provide a mechanism to identify and send notifications about issues that are found in audit logs. |
{: caption="Table 1. Requirements" caption-side="bottom"}

## Components
{: #components}

The following table outlines the products or services that are used in the architecture for each aspect:

| Aspects | Architecture component | How the component is used |
| -------------- | -------------- | -------------- |
| Compute | [Virtual Servers for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers&interface=ui) | Application server and database Server nodes |
|  | [Bare Metal Servers for VPC](/docs/vpc?topic=vpc-about-bare-metal-servers) | Application server and database server nodes (option) |
| Storage | [VPC File Storage](/docs/vpc?topic=vpc-file-storage-vpc-about) | File storage used by SAP Application Servers |
|  | [VPC Block Storage](/docs/vpc?topic=vpc-block-storage-about) | File storage used by Db2 Database Servers |
| Networking | [Virtual Private Endpoint (VPE)](/docs/vpc?topic=vpc-about-vpe) | For private network access to Cloud Services, for example, Key Protect, IAM, and so on. |
|  | [Public gateway](/docs/vpc?topic=vpc-about-public-gateways) | For secure client access to the high-performance computing (HPC) environment over the Internet |
|  | [Direct Link](/docs/dl?topic=dl-dl-about) | For private, dedicated connectivity between on-premises and cloud HPC resources |
| Security | [Identity and Access Management](https://cloud.ibm.com/iam/overview){: external} | Identity and Access Management |
|  | [Key protect](/docs/key-protect?topic=key-protect-about) or [Hyper Protect Crypto Services](/docs/hs-crypto?topic=hs-crypto-get-started) | Hardware security module (HSM) and Key Management Service |
|  | [Secrets Manager](/docs/secrets-manager?topic=secrets-manager-getting-started) | Certificate and secrets management |
| Resiliency | [Pacemaker](/https://clusterlabs.org/projects/pacemaker) | The Pacemaker software manages failures of application or database server nodes by managing failover to a surviving cluster node (VSI or bare metal) |
| Service Management | [{{site.data.keyword.Bluemix_notm}} monitoring](/docs/monitoring?topic=monitoring-about-monitor) | Operational monitoring |
|  | [{{site.data.keyword.Bluemix_notm}} Logs](/docs/cloud-logs) | For all {{site.data.keyword.Bluemix_notm}} related Logs |
|  | [Event Routing](/docs/cloud-logs?topic=cloud-logs-migration-tutorial-at-option3) | For all {{site.data.keyword.Bluemix_notm}} events  |
{: caption="Components" caption-side="bottom"}
