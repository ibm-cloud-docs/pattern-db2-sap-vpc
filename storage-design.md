---

copyright:
  years: 2025
lastupdated: "2025-04-14"

subcollection: pattern-db2-sap-vpc

keywords: storage, disk, database, volume, block storage

---

{{site.data.keyword.attribute-definition-list}}

# Storage design
{: #storage-design}

This pattern is built within an IBM Cloud Virtual Private Cloud (VPC) environment. For more information, see [IBM Cloud® Virtual Private Cloud (VPC) Infrastructure environment introduction](/docs/sap?topic=sap-vpc-env-introduction).

## Block storage 
{: #storage-design-block}

Block storage is provided with your virtual servers and uses input and output operations per second (IOPS) to determine storage needs. This option is ideal for storage-intensive applications with high I/O needs, such as an OS, database, and application software. 

Try not to use the boot volume of the VSI to provide block storage for both Db2 and SAP and add as separate volumes.{: note}

All block storage is selected based on capacity (GB) and performance (IOPS) measurements. It's must meet a specific SAP workload. IOPS values are measured based on 16 KB blocksize with a 50-50 read/write mix. To achieve a maximum I/O throughput, it's advisable to look at the tier and custom profiles available for storage and find the optimal combination of blocksize and IOPS.

Storage volumes differ in performance that depends on the IOPS tier. You can select among 3, 5, and 10 IOPS/GB. For more information, see [Tiered IOPS profiles](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#tiers). You can also select a [custom value](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#custom) in GB and IOPS, that is based on the blocksize of the storage. If you need more than the initially provisioned storage in your server, you can attach more volumes to it. Contact [IBM Cloud Support](/docs/account?topic=account-using-avatar#getting-support) for extension options if the attached storage is insufficient for your workload.

Shared Storage

Block storage can be detached and attached to other servers at any time, but, only to one server at the same time.

No shared storage for servers is available in VPC at the time of writing.
