---

copyright:
  years: 2025
lastupdated: "2025-03-21"

subcollection: pattern-db2-sap-vpc

keywords:

---

# Architecture decisions for resiliency
{: #resiliency-architecture}

The following sections summarize the resiliency architecture decisions for workloads deployed on IBM Cloud VPC infrastructure.

## Architecture decisions for high availability
{: #high-availability}

| Architecture decision | Requirement | Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| High Availability Deployment | * Ensure availability of resources if outages occur. \n * Support SLA targets for availability. | - Single zone, single region \n - Multi zone, single region \n - Multi-zone, multi region | text | text|
| High Availability Infrastructure | * Ensure availability of infrastructure resources if outages occur. \n * Support SLA targets for infrastructure availability. | text | text | text|
| High Availability Application and Database | * Ensure availability of application resources if outages occur. \n * Support SLA targets for application availability. | text | text | text|
{: caption="Table 1. High availability architecture decisions" caption-side="bottom"}

## Architecture decisions for backup and restore
{: #backup-and-restore}

| Architecture decision | Requirement | Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Infrastructure backup  | Backup images to enable recovery. | text | text | text |
| Database backup | Create transaction-consistent database backups to enable recovery of database tier if unplanned outages occur. |text | text | text |
| File Backup | Create file system backups |text | text | text |
| Backup Automation | Schedule regular database backups based on RPO requirements to enable data recovery if unplanned outages occur. |text | text | text |
{: caption="Table 2. Backup and restore architecture decisions" caption-side="bottom"}

## Architecture decisions for disaster recovery
{: #disaster recovery}

| Architecture decision | Requirement | Option | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- | -------------- |
| Disaster Recovery - application | Application disaster recovery capability in secondary region to meet RTO/RPO requirements| text | text | text |
| Disaster Recovery - database                        | Database recovery capability in secondary region | text | Continuous replication of data from a primary to a secondary system in a separate region, including in-memory loading, system replication facilitates rapid failover in the event of a disaster|
| Disaster Recovery - infrastructure | Infrastructure disaster recovery capability in secondary region to meet RTO/RPO requirements| text | text | text |
{: caption="Table 2. Disaster recovery architecture decisions" caption-side="bottom"}
