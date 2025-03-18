---
copyright:
  years: 2025
lastupdated: "2025-03-18"

subcollection: pattern-db2-sap-vpc 

keywords:
---
{{site.data.keyword.attribute-definition-list}}


The Highly Available SAP with Db2 on IBM Cloud VPC deployment guide provides an IBM® solution design for the deployment of a highly available Db2 database on IBM Cloud® Virtual Private Cloud (VPC) using SUSE Linux. This solution uses Pacemaker - an open source high-availability cluster resource manager - to deliver high availabilty to the Db2 database.

This guide, in turn, can form the basis of a highly available SAP deployment also using the Pacemaker software.

This document is intended to:

* Accelerate and simplify solution design by providing a standard IBM Cloud deployment architecture reference following the IBM Architecture Framework.
* Provide a solution design, with diagrams, component architecture decisions along with rationale for cloud component selection to meet enterprise requirements.
* Provide links to key documentation sources describing the deployment steps required to build these solutions.

This document is not intended to replace or supercede any documentation for either Db2 or SAP.  Rather, this document brings these diverse documentation resources together to deliver a highly available solution.

Following the Architecture Framework, the Highly Available SAP with Db2 on IBM Cloud VPC pattern covers design considerations and architecture decisions for the following aspects and domains:

- Compute: Virtual and Bare Metal Servers
- Storage: Primary Storage
- Networking: xxxx, yyyy, zzzz
- Resiliency: High Availability
- Service Management: Monitoring, Logging, Auditing, Alerting

The Architecture Framework provides a consistent approach to design cloud solutions by addressing requirements across a set of "aspects" and "domains", which are technology-agnostic architectural areas that need to be considered for any enterprise solution. For more details, see [Introduction to the Architecture Framework](/docs/architecture-framework).
