---
copyright:
  years: 2025
lastupdated: "2025-05-01"

subcollection: pattern-db2-sap-vpc 

keywords:
---
{{site.data.keyword.attribute-definition-list}}

# Overview
{: #overview}

The objective of this pattern is to provide an {{site.data.keyword.IBM_notm}} solution design and deployment guide for the highly available SAP with Db2 on {{site.data.keyword.vpc_full}} using SUSE Linux Enterprise server or Red Hat Enterprise Linux. This solution uses Pacemaker, an open source high-availability cluster resource manager to deliver high availability to the Db2 database. 

## Pattern objectives 
{: db2-sap-objectives}

This document is intended to:

* Accelerate and simplify solution design by providing a standard {{site.data.keyword.cloud_notm}} deployment architecture reference following the {{site.data.keyword.IBM_notm}} Architecture Framework.
* Provide a solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.
* Provide links to key documentation sources describing the deployment steps required to build these solutions.

## Pattern details 
{: db2-pattern-details}

This document is not intended to replace or supersede any documentation for either Db2 or SAP. Rather, this document brings these diverse documentation resources together to deliver a highly available solution.

Following the Architecture Framework, the highly available SAP with Db2 on {{site.data.keyword.vpc_short}} pattern covers design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual and bare metal servers
- Storage: Primary storage
- Networking: In-cloud and cloud-to-enterprise connectivity including hostname resolution
- Resiliency: High availability
- Service management: Monitoring, Logging, Auditing, Alerting

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. For more information, see [Introduction to the Architecture Framework](/docs/architecture-framework).
