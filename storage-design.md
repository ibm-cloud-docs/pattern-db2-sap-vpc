---

copyright:
  years: 2025
lastupdated: "2025-04-23"

subcollection: pattern-db2-sap-vpc

keywords: storage, disk, database, volume, block storage

---

{{site.data.keyword.attribute-definition-list}}

# Storage design
{: #storage-design}

SAP solutions want to make sure that there is enough available storage to accommodate the existing environment and allow for more data growth for primary, backup and archive storage. You need to choose the appropriate SAP-certified profile to meet primary storage and growth requirements for workloads.

This pattern is built within an IBM Cloud Virtual Private Cloud (VPC) environment. For more information, see [IBM Cloud Virtual Private Cloud (VPC) Infrastructure environment introduction](/docs/sap?topic=sap-vpc-env-introduction).

## Block storage 
{: #storage-design-block}

Block storage is provided with your virtual servers and uses input and output operations per second (IOPS) to determine storage needs. This option is ideal for storage-intensive applications with high I/O needs, such as an OS, database and application software. 

Add separate volumes to provide block storage for Db2 and SAP instead of using the boot volume of the VSI. {: note}

All block storage is selected based on capacity (GB) and performance (IOPS) measurements. It must meet a specific SAP workload. IOPS values are measured based on 16 KB blocksize with a 50–50 read and write mix. To achieve a maximum I/O throughput, its advised to look at the tier and custom profiles available for storage and find the optimal combination of blocksize and IOPS.

Storage volumes differ in performance that depends on the IOPS tier. You can select among 3, 5, and 10 IOPS/GB. For more information, see [Tiered IOPS profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#tiers). You can also select a [custom value](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#custom) in GB and IOPS, that is based on the blocksize of the storage. If you need more than the initially provisioned storage in your server, you can attach more volumes to it. Contact [IBM Cloud Support](/docs/account?topic=account-using-avatar#getting-support) for extension options if the attached storage is insufficient for your workload.
