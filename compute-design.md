---

copyright:
  years: 2025
lastupdated: "2025-04-23"

subcollection: pattern-db2-sap-vpc

keywords: Intel, virtual machine, VSI, server, host, compute

---

{{site.data.keyword.attribute-definition-list}}

# Compute design
{: #compute-design}

The basis of any cloud solution is the compute resources that run the applications and any supporting services that the application requires. This topic introduces the considerations for the compute resources needed to support the {{site.data.keyword.IBM_notm}} Db2 database layer of the overall solution as well as the compute resources needed to support the SAP application layer.

This pattern is built within an {{site.data.keyword.Bluemix_notm}} Virtual Private Cloud (VPC) environment. For more information, see [{{site.data.keyword.Bluemix_notm}} Virtual Private Cloud (VPC) infrastructure environment](/docs/sap?topic=sap-vpc-env-introduction).

The pattern requires a pair of servers to support the Db2 high availability disaster recovery (HADR) primary and standby database and a separate pair of servers to support the SAP components. The SAP component options are Virtual Server Instances (VSIs) or Bare Metal servers. For more information, see:

* Virtual Server Instances (VSIs)

    * [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).

    * [{{site.data.keyword.Bluemix_notm}} Intel Virtual Servers on VPC Infrastructure](/docs/sap?topic=sap-fast-path-site-map-intel-vs-gen2).

* Bare Metal Servers

    * [About Bare Metal Servers for VPC](/docs/vpc?topic=vpc-about-bare-metal-servers).

    * [{{site.data.keyword.Bluemix_notm}} Intel Bare Metal Servers on VPC Infrastructure](/docs/sap?topic=sap-fast-path-site-map-intel-bm-vpc).

The database servers that run {{site.data.keyword.IBM_notm}} Db2 must be sized to support the workload requirements. For more information, see [System requirements for {{site.data.keyword.IBM_notm}} Db2 for Linux, UNIX, and Windows](https://www.ibm.com/support/pages/system-requirements-ibm-db2-linux-unix-and-windows){: external}.

Only certain server profiles, both VSIs and Bare Metal servers, are supported for running SAP. For more information, see [Infrastructure certified for SAP](/docs/sap?topic=sap-iaas-offerings).
{: note}
