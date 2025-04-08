---

copyright:
  years: 2025
lastupdated: "2025-04-08"

subcollection: pattern-db2-sap-vpc

keywords:

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for resiliency for the SAP on VPC pattern.

## Architecture decisions for high availability
{: #ha-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| High Availability Db2 | Provide high availability for Db2 databases | IBM Db2 HADR | Enabling Db2 HADR addresses Db2 outage reduction due to planned maintenance, failures, faults and disasters |
| High Availability infrastructure | Provide 99.95% availability for infrastructure | Redundant VSIs in an SAP Scale-out deployment with {{site.data.keyword.alb_full}} (ALB) solution on a single-zone can provide an application SLA of 99.95% \n Auto Scale for VPC (optional)  | Minimize cost, implementation and maintenance complexity, potential latency and maximize value with {{site.data.keyword.IBM}} solutions.|
{: caption="Architecture decisions for High Availability (HA)" caption-side="bottom"}

## Architecture decisions for disaster recovery
{: #da-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| Disaster Recovery for Db2 | Db2 Disaster recovery capability in secondary region | IBM Db2 HADR | Continuous replication of data from a primary to a secondary system in a separate region |
| Disaster Recovery for application and infrastructure | Disaster recovery capability in secondary region | Veeam | Support for Db2 databases \n DR capability for RPO \< 15 min, RTO \< 4 hours. Veeam service seamlessly integrates directly with {{site.data.keyword.vpc_short}} to help achieve high availability. This service provides recovery points and time objectives for SAP application servers. Controls both the backups and restores of all virtual machines (VMs) for SAP applications directly from the Veeam console. |
{: caption="Architecture decisions for Disaster Recovery (DR)" caption-side="bottom"}

## Architecture decisions for backup and restore
{: #backup-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| Backup (Db2) | Back up of Db2 data | * IBM Db2 database backup \n * IBM Db2 storage snapshot | Minimize cost and operational ease by using Db2 Native tools |
| Backup (SAP) | Back up of NetWeaver data | * SAP NetWeaver(NW) file level backup \n * SAP NetWeaver(NW) storage snapshot | Use SAP Native tools like DBACOCKPIT and Backint. |
| Backup for VPC infrastructure | Infrastructure backups | Veeam for snapshot backups at VM/BM level. |The backups are integrated with {{site.data.keyword.Bluemix_notm}} native backup solution of Veeam by using the API that's available to store the backup in the {{site.data.keyword.cos_full_notm}}.
{: caption="Architecture decisions for backup and restore" caption-side="bottom"}
