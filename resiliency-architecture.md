---

copyright:
  years: 2025
lastupdated: "2025-04-23"

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
| High availability Db2 | Provide high availability for Db2 databases | {{site.data.keyword.IBM_notm}} Db2 HADR | Enabling Db2 HADR addresses Db2 outage reduction due to planned maintenance, failures, faults, and disasters. |
| High availability infrastructure | Provide 99.95% availability for infrastructure | Redundant VSIs in an SAP scale-out deployment with {{site.data.keyword.alb_full}} (ALB) solution on a single-zone can provide an application SLA of 99.95% \n Auto scale for VPC (optional)  | Minimize cost, implementation, maintenance complexity, potential latency and maximize value with {{site.data.keyword.IBM}} solutions.|
{: caption="Architecture decisions for high availability (HA)" caption-side="bottom"}

## Architecture decisions for disaster recovery
{: #da-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| Disaster recovery for Db2 | Db2 disaster recovery capability in secondary region | {{site.data.keyword.IBM_notm}} Db2 HADR | Continuous replication of data from a primary to a secondary system in a separate region |
| Disaster recovery for application and infrastructure | Disaster recovery capability in secondary region | Veeam | Support for Db2 databases \n DR capability for RPO \< 15 min, RTO \< 4 hours. Veeam service seamlessly integrates directly with {{site.data.keyword.vpc_short}} to help achieve high availability. This service provides recovery points and time objectives for SAP application servers. Controls both the backups and restores of all virtual machines (VMs) for SAP applications directly from the Veeam console. |
{: caption="Architecture decisions for disaster recovery (DR)" caption-side="bottom"}

## Architecture decisions for backup and restore
{: #backup-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| Backup (Db2) | Backup of Db2 data | * {{site.data.keyword.IBM_notm}} Db2 database backup \n * {{site.data.keyword.IBM_notm}} Db2 storage snapshot | Minimize cost and operational ease by using Db2 native tools. |
| Backup (SAP) | Back up of SAP NetWeaver data | * SAP NetWeaver file level backup \n * SAP NetWeaver storage snapshot | Use SAP native tools like DBACOCKPIT and Backint. |
| Backup for VPC infrastructure | Infrastructure backups | Veeam for snapshot backups at the virtual machine (VM) and bare metal level. | The backups are integrated with {{site.data.keyword.Bluemix_notm}} native backup solution of Veeam by using the API that's available to store the backup in the {{site.data.keyword.cos_full_notm}}.
{: caption="Architecture decisions for backup and restore" caption-side="bottom"}
