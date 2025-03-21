---

copyright:
  years: 2025
lastupdated: "2025-03-21"

keywords: SAP, Db2, Pacemaker, SUSE, SLES, SUSE Linux, High Availability, cluster

subcollection: pattern-db2-sap-vpc 

authors:
  - name: John Easton
    url: https://linkedin.com/in/johnpeaston
  - name: "John Easton"
    url: "linkedIn profile URL"

version: 1.0

deployment-url: url

# docs: https://cloud.ibm.com/docs/solution-guide

# image_source: https://github.com/terraform-ibm-modules/module/reference-architectures/xxx.svg

# related_links:
#  - title: 'Highly Available SAP with Db2 on IBM Cloud VPC'
#    url: 'https://url.com'
#    description: 'Deploying highly available SAP and Db2 on IBM Cloud VPC using SUSE Linux.'
#  - title: 'Related or follow-on architectures'
#    url: 'https://cloud.ibm.com/docs/pattern-sap-on-vpc'
#    description: 'An IBM® solution design for the deployment of SAP on IBM Cloud® Virtual Private Cloud (VPC).'

use-case: Data resiliency, Enterprise resource planning
industry: FinancialSector, Manufacturing, Retail, Industrials
compliance: ISOIEC27001
content-type: reference-architecture

production: false

---

{{site.data.keyword.attribute-definition-list}}

# Highly Available SAP with Db2 on IBM Cloud VPC
{: #pattern-db2-sap-vpc}
{: toc-content-type="reference-architecture"}
{: toc-industry="FinancialSector, Manufacturing, Retail, Industrials"}
{: toc-use-case="Data resiliency, Enterprise resource planning"}
{: toc-compliance="ISOIEC27001"}
{: toc-version="1.0"}



:information_source: **Tip:** For more information about this template, see [Creating reference architectures](https://test.cloud.ibm.com/docs-internal/writing?topic=writing-reference-architectures).

Include a short description, summary, or overview in a single paragraph that follows the title.

After the introduction, include a summary of the typical use case for the architecture. The use case might include the motivation for the architecture composition, business challenge, or target cloud environments.

## Architecture diagram
{: #architecture-diagram}

Include the architecture diagram SVG file that was created by using drawio and the IBM2 library.

![Enter image alt text here.](images/ha-db2-sap-architecture.drawio.svg "Title text that shows on hover here"){: caption="Figure 1. A description that prints on the page" caption-side="bottom"}

If you have a list or text to describe the diagram, include it here.

## Design concepts
{: #design-concepts}

Customize the design requirement heat map template image and highlight the scope of the architecture. Publishing in IBM Cloud Docs requires a caption to meet accessibility requirements.

![Enter image alt text here.](images/ha-db2-sap-heat-map.drawio.svg "Title text that shows on hover here"){: caption="Figure 2. A description that prints on the page" caption-side="bottom"}

For more information about creating a design requirements heat map image, see [Design requirements heat map](https://test.cloud.ibm.com/docs/architecture-framework?topic=architecture-framework-heat-map).


## Requirements
{: #requirements}

Update the following table with requirements for this architecture. Introduce the table with a sentence. For example, "The following table outlines the requirements that are addressed in this architecture."

| Aspect | Requirements |
| -------------- | -------------- |
| Compute            | Provide properly isolated compute resources with adequate compute capacity for the applications. |
| Storage            | Provide storage that meets the application and database performance requirements. |
| Networking         | Deploy workloads in isolated environment and enforce information flow policies. \n Provide secure, encrypted connectivity to the cloud’s private network for management purposes. \n Distribute incoming application requests across available compute resources. \n Support failover of application to alternate site in the event of planned or unplanned outages \n Provide public and private DNS resolution to support use of hostnames instead of IP addresses. |
| Security           | Ensure all operator actions are executed securely through a bastion host. \n Protect the boundaries of the application against denial-of-service and application-layer attacks. \n Encrypt all application data in transit and at rest to protect from unauthorized disclosure. \n Encrypt all backup data to protect from unauthorized disclosure. \n Encrypt all security data (operational and audit logs) to protect from unauthorized disclosure. \n Encrypt all data using customer managed keys to meet regulatory compliance requirements for additional security and customer control. \n Protect secrets through their entire lifecycle and secure them using access control measures. |
| Resiliency         | Support application availability targets and business continuity policies. \n Ensure availability of the application in the event of planned and unplanned outages. \n Provide highly available compute, storage, network, and other cloud services to handle application load and performance requirements. \n Backup application data to enable recovery in the event of unplanned outages. \n Provide highly available storage for security data (logs) and backup data. \n Automate recovery tasks to minimize down time |
| Service Management | Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. \n Generate alerts/notifications about issues that might impact the availability of applications to trigger appropriate responses to minimize down time. \n Monitor audit logs to track changes and detect potential security problems. \n Provide a mechanism to identify and send notifications about issues found in audit logs. |
{: caption="Table 1. Requirements" caption-side="bottom"}

## Components
{: #components}

Update the following table below with components that are unique to this architecture. Introduce the table with a sentence. For example, "The following table outlines the products or services used in the architecture for each aspect."

| Aspects | Architecture components | How the component is used |
| -------------- | -------------- | -------------- |
| Compute | PowerVS | Web, App, and database servers |
| Storage | PowerVS | Database servers shared storage for RAC |
|  | VPC Block Storage | Web app storage if neededt |
| Networking | VPC Virtual Private Network (VPN) | Remote access to manage resources in private network |
|  | Virtual Private Gateway & Virtual Private Endpoint (VPE) | For private network access to Cloud Services, e.g., Key Protect, COS, etc. |
|  | VPC Load Balancers | Application Load Balancing for web servers, app servers, and database servers |
|  | Public Gateway | For web server access to the internet |
| Security | IAM | IBM Cloud Identity & Access Management |
|  | BYO Bastion Host on VPC VSI | Remote access with Privileged Access Management |
|  | Key protect or HPCS | Hardware security module (HSM) and Key Management Service |
|  | Secrets Manager | Certificate and Secrets Management |
| Resiliency | PowerVS | Multiple PowerVS on separate physical servers with VM and Storage anti-affinity policy |
| Service Management | IBM Cloud Monitoring | Apps and operational monitoring |
|  | IBM Log Analysis | Apps and operational logs |
|  | Activity Tracker Event Routing | Audit logs |
| Other  use if there is  additional aspect(s)  Name Aspect | Cell content | Cell content |
{: caption="Table 2. Components" caption-side="bottom"}

## Compliance
{: #compliance}

_Optional section._ Feedback from users implies that architects want only the high-level compliance items and links off to control details that team members can review. Include the list of control profiles or compliance audits that this architecture meets. For controls, provide "learn more" links to the control library that is published in the IBM Cloud Docs. For audits, provide information about the compliance item.

## Next steps
{: #next-steps}

_Optional section._ Include links to your deployment guide or next steps to get started with the architecture.


:exclamation: **Important:** Rename this file `<architecture-name>.md`. For deployable architectures, `<architecture-name>` is the same as the deployable architecture name.
