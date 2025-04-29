---

copyright:
  years: 2025
lastupdated: "2025-04-25"

subcollection: pattern-db2-sap-vpc

keywords: resilience, high avalability, disaster recovery, Pacemaker, cluster, protection

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}

The highly available SAP with {{site.data.keyword.IBM_notm}} Db2 on {{site.data.keyword.Bluemix}} VPC pattern delivers increased resilience over single server deployments of either {{site.data.keyword.IBM_notm}} Db2 or SAP. The Virtual Server Instances (VSIs) or bare metal servers are clustered together to avoid the host being a single point of failure. This pattern delivers a solution with {{site.data.keyword.IBM_notm}} Db2 and SAP properties.

## {{site.data.keyword.IBM_notm}} Db2
{: #ibm-cloud-db2-resiliency}

Resilience for the {{site.data.keyword.IBM_notm}} Db2 database is delivered by using the following key components:

 * Two {{site.data.keyword.IBM_notm}} Db2 cluster nodes are connected to one another by the {{site.data.keyword.Bluemix_notm}} network. These nodes are Virtual Server Instances (VSIs) running within a Virtual Private Cloud (VPC).

* Separate disk storage attached to each of the VSIs that contains the Db2 database. The Db2 HADR does not operate with shared storage.

    Databases are key repositories of business information requiring them to be both performant and highly available. The Db2 databases are made highly available using Db2 high availability disaster recovery (HADR). This is a data replication feature. With HADR there are two separate Db2 database servers: a primary and a standby with all clients connected to the primary server. Database transactions are transferred to the second, a standby database server over the network. The standby server updates its local database by using these transactions to be kept synchronized with the primary server. If a failure of the primary database server occurs, the standby database server takes over the workload.  

* A floating, virtual IP address that allows clients to connect to the Db2 database service no matter which cluster node it's running on.

 * Use of a Qdevice host or cloud fencing agent to avoid split-brain scenarios when the cluster nodes can't communicate with one another.

* Active-passive failover and failback of resources from one cluster node to the other if the active host fails.

## SAP 
{: #sap-resiliency} 

Resilience for the SAP application layer of the solution is delivered by using the following key components:

* Two SAP cluster nodes connected to one another by the {{site.data.keyword.Bluemix_notm}} network. These nodes are Virtual Server Instances (VSIs) running within a Virtual Private Cloud (VPC).

 * {{site.data.keyword.IBM_notm}} VPC File Storage attached to each of the VSIs that run the SAP components. This shared file system is mounted by using the Network File System (NFS) protocol to facilitate sharing of data between the SAP components.

    The two SAP servers communicate by using an Enqueue Server. The Enqueue Server on the primary nodes transmits replication data to the Enqueue Replication Server on the standby system. This stores the data in a shadow enqueue table residing in shared memory. In case the primary Enqueue Server fails, the shadow enqueue table on the Enqueue Replication Server is used to rebuild the tables and data structures for the recovered Enqueue Server that is started on the same node. The Enqueue Replication Server stops after transferring the data to the recovered Enqueue Server.

* A floating, virtual IP address that allows clients to connect to the SAP service no matter which cluster node it's running on.

* Use of a cloud fencing agent to avoid split-brain scenarios when the cluster nodes can't communicate with one another.

* Active-passive failover and failback of resources from one cluster node to the other if the active host fails.

    Failure detection and automation of the recovery processing requires a cluster manager such as [Pacemaker](https://clusterlabs.org/projects/pacemaker/){: external}. Pacemaker monitors the two servers and initiates the failover to the standby server when required. There are two Pacemaker clusters, one to support the {{site.data.keyword.IBM_notm}} Db2 database cluster and one to support the SAP cluster.

The high level [reference architecture](/images/sap-db2-vpc-detailedHLA.drawio.svg) for this solution provides a view of SAP as a single entity. In reality, the SAP environment consists of multiple separate components:

- ABAP Central Services (ASCS)
- Enqueue Replication Server (ERS) for the ASCS
- Primary Application Server (PAS)

Each of these components needs redundancy to avoid being a single point of failure for the SAP application. The detail of this is illustrated in the following diagram:

![SAP resilience approach for individual SAP components](/images/sap-ha-scenario.drawio.svg "SAP resilience approach for individual SAP components"){: caption="SAP resilience approach for individual SAP components" caption-side="bottom"}

It is recommended to engage an SAP specialist when designing the high availability approach for an SAP environment to ensure that each SAP component is appropriately protected to deliver the SLAs that are required by the business users of SAP.

## Single zone and multizone choices
{: #az-resiliency} 

The highly available SAP with {{site.data.keyword.IBM_notm}} Db2 on {{site.data.keyword.Bluemix_notm}} VPC pattern can be deployed within a single {{site.data.keyword.Bluemix_notm}} region. It can be deployed with all of the cluster nodes within a single availability zone (AZ) as shown in the following diagram:

![Single AZ resilience approach for highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC](/images/sap-db2-vpc-HLA-1AZ+sap.drawio.svg "Single AZ resilience approach for highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPCs"){: caption="Single AZ resilience approach for highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC" caption-side="bottom"}

For protection against the unlikely failure of an availability zone, there is also the option to split the Pacemaker clusters across two separate availability zones.  This is illustrated in the following diagram:

![Dual AZ resilience approach for highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC](/images/sap-db2-vpc-HLA-2AZ+sap.drawio.svg "Dual AZ resilience approach for highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}}VPCs"){: caption="Dual AZ resilience approach for highly available SAP with Db2 on {{site.data.keyword.Bluemix_notm}} VPC" caption-side="bottom"}

## Disaster recovery
{: #dr-resiliency} 

High availability is different than disaster recovery though many aspects are common across both. Solutions that need to provide disaster recovery are recommended to review the following documentation: [Planning disaster recovery for SAP solutions on {{site.data.keyword.Bluemix_notm}}](/docs/sap?topic=sap-disaster-recovery-design-considerations-overview).
