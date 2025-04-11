---

copyright:
  years: 2025
lastupdated: "2025-04-11"

subcollection: pattern-db2-sap-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for compute
{: #compute-decisions}

Review the following compute architectural decisions for SAP running on IBM Cloud VPC:

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
|Bare Metal (NetWeaver) |Target environment to match specific workload requirements and be SAP certified |Bare Metal on VPC| Choose bare metal on VPC for larger SAP deployments with higher SAPS processing requirements. Limited to low memory requirements. If memory is a concern, VSIs are beneficial. |
|Virtual Servers (Netweaver)        |Target environment to match workload requirements and be SAP certified          | VSI on VPC        |Choose VSIs for a more cost-effective approach and when deployments don't require the larger number of SAPs bare metal can provide. VSI profiles can satisfy larger memory requirements than bare metal.|
|Bare Metal (HANA)             |Target environment to match workload requirements and be SAP certified          |Bare Metal on VPC |Choose bare metal on VPC for larger SAP requirements. However, memory limitations exist on the bare metal server profiles for HANA. If memory is a concern, VSIs are beneficial. |
|Virtual Servers (HANA)        |Target environment to match workload requirements and be SAP certified          |VSI on VPC        |VPC VSIs can cover 90% of customer use cases for the HANA database service. VSIs can accommodate a high number of SAPS and higher memory over bare metal. If HANA in-memory requirements can't be met by using VSI profiles, then Power Virtual Servers are the preferred choice and is covered in the SAP on Power Virtual Server documentation.|
{: caption="Architecture decisions for compute" caption-side="bottom"}
