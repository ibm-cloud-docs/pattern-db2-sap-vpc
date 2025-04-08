---

copyright:
  years: 2025
lastupdated: "2025-04-08"

subcollection: pattern-db2-sap-vpc

keywords:

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following link will take you to the resiliency architectural decisions for SAP running on IBM Cloud VPC:

[Architecture decisions for resiliency](/docs/pattern-sap-on-vpc?topic=pattern-sap-on-vpc-resiliency-decisions)

The following sections summarize the resiliency architecture decisions for resiliency for the SAP on VPC pattern.

## Architecture decisions for high availability
{: #ha-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| High Availability HANA | Provide 99.95% availability for HANA database | HANA System Replication (HSR) | Enabling HANA HSR addresses SAP HANA outage reduction due to planned maintenance, faults, and disasters. It supports a recovery point objective (RPO) of 0 seconds and a recovery time objective (RTO) measured in minutes. |
| High Availability infrastructure | Provide 99.95% availability for infrastructure | * Redundant VSIs in an [SAP Scale-out](/docs/sap?topic=sap-refarch-hana-scaleout#network-layout-for-scale-out-configurations-2) deployment with {{site.data.keyword.alb_full}} (ALB) solution on a single-zone can provide an application SLA of 99.95% \n Auto Scale for VPC (optional)  | Minimize cost, implementation and maintenance complexity, potential latency and maximize value with {{site.data.keyword.IBM}} solutions.|
{: caption="Architecture decisions for High Availability (HA)" caption-side="bottom"}

## Architecture decisions for disaster recovery
{: #da-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| Disaster Recovery for application and infrastructure | Disaster recovery capability in secondary region | Veeam | * Support for databases (HANA, Oracle, or MSSQL). \n * DR capability for RPO \< 15 min, RTO \< 4 hours. Veeam service seamlessly integrates directly with {{site.data.keyword.vpc_short}} to help achieve high availability. This service provides recovery points and time objectives for SAP application servers. Controls both the backups and restores of all virtual machines (VMs) for SAP applications directly from the Veeam console. |
| Disaster Recovery for HANA | HANA disaster recovery capability in secondary region | Hana System Replication | Continuous replication of data from a primary to a secondary system in a separate region, including in-memory loading. System replication facilitates rapid failover due to a disaster |
{: caption="Architecture decisions for Disaster Recovery (DR)" caption-side="bottom"}

## Architecture decisions for backup and restore
{: #backup-arch}

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| Backup (SAP) | Back up of NetWeaver and HANA data | * SAP NetWeaver(NW) file level backup \n * SAP NetWeaver(NW) storage snapshot \n * SAP HANA file level backup \n * SAP HANA storage snapshot | Minimize cost and operational ease by using SAP Native tools like DBACOCKPIT, HANACOCKPIT, and Backint. The tools are used to take SAP Database backup. |
| Backup for VPC infrastructure | Infrastructure backups | Veeam for snapshot backups at VM/BM level. |The backups are integrated with {{site.data.keyword.Bluemix_notm}} native backup solution of Veeam by using the API that's available to store the backup in the {{site.data.keyword.cos_full_notm}}.
{: caption="Architecture decisions for backup and restore" caption-side="bottom"}
