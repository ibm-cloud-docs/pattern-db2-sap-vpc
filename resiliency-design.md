---

copyright:
  years: 2025
lastupdated: "2025-03-24"

subcollection: pattern-db2-sap-vpc

keywords: resilience, high avalability, disaster recovery, Pacemaker, cluster, protection

---

{{site.data.keyword.attribute-definition-list}}

# Resiliency design
{: #resiliency-design}

The Highly Available SAP with {{site.data.keyword.IBM&reg}} Db2 on {{site.data.keyword.Bluemix}} VPC pattern delivers increased resilience over single server deployments of either IBM Db2 or SAP. The VSIs or Bare Metal servers are clustered together to avoid the host being a single point of failure. This pattern delivers a solution with the following properties:

* For {{site.data.keyword.IBM_notm}} Db2

    * Two IBM Db2 cluster nodes connected to one another by the IBM Cloud network.  These nodes are Virtual System Instances (VSIs) running within a Virtual Private Cloud (VPC).

    * Separate disk storage attached to each of the VSIs that contains the DB2 database.  Note that Db2 does not operate with shared storage.

    Databases are key repositories of business information requiring them to be both performant and highly available.  The Db2 databases are made highly available using Db2 High Availability and Disaster Recovery (HADR).  This is a data replication feature.  With HADR there two separate Db2 database servers: a primary and a standby with all clients connected to the primary server. Database transactions are written to log files and these log files are transferred to the second (standby) database server over the network. The standby server updates the local database using the transferred log files to be kept synchronised with the primary server.  In the event of a failure of the primary database server, the standby database server takes over the workload.  

    * A floating, virtual IP address that allows clients to connect to the Db2 database service no matter which cluster node it is running on.

    * Use of a cloud fencing agent to avoid split-brain scenarios when the cluster nodes cannot communicate with one another.

    * Active-passive failover and failback of resources from one cluster node to the other if the active host fails.

* For SAP 

    * Two SAP cluster nodes connected to one another by the IBM Cloud network.  These nodes are Virtual System Instances (VSIs) running within a Virtual Private Cloud (VPC).

    * {{site.data.keyword.IBM_notm}} VPC File Storage attached to each of the VSIs that run the SAP components.  This shared filesystem is mounted using the NFS protocol to facilitate sharing of data between the SAP components.

    * A floating, virtual IP address that allows clients to connect to the SAP service(s) no matter which cluster node it is running on.

    * Use of a cloud fencing agent to avoid split-brain scenarios when the cluster nodes cannot communicate with one another.

    * Active-passive failover and failback of resources from one cluster node to the other if the active host fails.

Failure detection and automation of the recovery processing requires a cluster manager such as [Pacemaker](https://clusterlabs.org/projects/pacemaker/).  This monitors the two servers and initiates the failover to the standby server when required.  There are two Pacemaker clusters.  One to support the {{site.data.keyword.IBM_notm}} Db2 database cluster and one to support the SAP cluster.

The Highly Available SAP with {{site.data.keyword.IBM_notm}} Db2 on {{site.data.keyword.Bluemix_notm}} VPC pattern can be deployed within a single IBM Cloud region.  It can be deployed with all of the cluster nodes within a single Availability Zone (AZ) as shown in the following diagram:

![Single AZ resilience approach for Highly Available SAP with Db2 on IBM Cloud VPC](/images/sap-db2-vpc-HLA-1AZ+sap.drawio.svg "Single AZ resilience approach for Highly Available SAP with Db2 on IBM Cloud VPCs"){: caption="Single AZ resilience approach for Highly Available SAP with Db2 on IBM Cloud VPC" caption-side="bottom"}

For protection against the unlikely failure of an Availability Zone, there is also the option to split the Pacemaker clusters across two separate Availability Zones.  This is illustrated in the following diagram:

![Dual AZ resilience approach for Highly Available SAP with Db2 on IBM Cloud VPC](/images/sap-db2-vpc-HLA-2AZ+sap.drawio.svg "Dual AZ resilience approach for Highly Available SAP with Db2 on IBM Cloud VPCs"){: caption="Dual AZ resilience approach for Highly Available SAP with Db2 on IBM Cloud VPC" caption-side="bottom"}

Solutions needing to also provide disaster recovery are recommended to review the documentation provided here:

[Planning Disaster Recovery for SAP solutions on IBM Cloud](/docs/sap?topic=sap-disaster-recovery-design-considerations-overview).
