---

copyright:
  years: 2025
lastupdated: "2025-11-18"

subcollection: pattern-db2-sap-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

The following table summarizes the networking architectural decisions for SAP using a Db2 database running on {{site.data.keyword.cloud_notm}} VPC:

| Architecture decision | Requirement | Decision | Rationale |
| -------------- | -------------- | -------------- | -------------- |
| Connectivity from cloud to enterprise         | Connectivity that is needed between client and {{site.data.keyword.Bluemix_short}}                                                                           | Redundant Direct Link Connect Connections                                        | Preferred depending on security requirements. More cost-efficient than Direct Link Dedicated                                                                                                              |
| Connectivity from Managed Service Providers   | Secure, encrypted connectivity between MSPs and {{site.data.keyword.Bluemix_short}}                                                                  | P2P VPN through VPC VPN Gateway                                                  | [VPN Gateway](/docs/vpc?topic=vpc-using-vpn) - securely connect Virtual Private Cloud (VPC) to another private network (site-to-site) for management purposes. \n A VPN gateway consists of two back-end instances for high availability in the same zone.      |
| Bring Your Own IP (BYOIP) and edge gateway                            | Capability needed for customers to provide isolation, security, and edge routing services                                    |{{site.data.keyword.vpc_full}} facilitates Bring Your Own IP(BYOIP) \n Edge gateways: Palo Alto, Fortinet, F5 with the client choice based on requirements                                             | Client can [bring their own subnet](/docs/vpc?topic=vpc-configuring-address-prefixes) IP address range to an {{site.data.keyword.vpc_full}} \n Edge gateway is client choice based on the requirements                                        |
| Network segmentation and isolation                | Ability to provide network isolation across workloads                                                                      | VPCs and subnets                                                                 | Native VPC isolation by using separate VPCs and subnets for production, nonproduction environments, and separation of workload                                                              |
| Cloud Native Connectivity (to cloud services) | Ability to connect to cloud services over the private network                                                              | Virtual Private Endpoints                                                        | Communicate with {{site.data.keyword.Bluemix_short}} services over the private network by using a virtual private endpoint (VPE)                                                                                     |
| Cloud landing-zone connectivity VPC to VPC                | Connect across multiple VPCs to {{site.data.keyword.Bluemix_short}} classic environments                                                             | Transit gateway \n Global transit gateway                                                                  | Use a transit gateway to connect separate VPCs (Edge, workload) and Classic (if needed). Global transit gateway to connect to environments in other regions for resiliency and data replication purposes. |
| Load balancing (Public)                       | Load balancing over the public network and across two regions if of an outage (disaster recovery) or failover to the other region. | Cloud Internet Services (CIS)                                                    | Public load balancing for resiliency needs from SAP best practices. CIS also provides DDoS services.                                                                                  |
| Load balancing (Private)                      | Load balancing workloads across multiple workload instances over the private network                                       | SAP Web Dispatcher \n {{site.data.keyword.Bluemix_short}} Application Load Balancer (ALB)                                                              | SAP Web Dispatcher forwards incoming HTTP and HTTPS requests to SAP application. \n The ALB loads balance inter-application server requests across hosts. The ALB is a floating IP with multiple subnets or servers that are attached to the backend pool. servers                                                                                                 |
| Domain Name System (DNS)                  | Ability to resolve DNS names on site                                                                                       | {{site.data.keyword.IBM}} continues to forward the DNS to client DNS Servers onsite          | This is the default option in the absence of a specific customer requirement to manage DNS                                                                                              |
{: caption="Architecture decisions for network" caption-side="bottom"}

## Resilient communication between enterprise networks and {{site.data.keyword.cloud_notm}} servers
{: #resilient-comms-to-cloud}

Most organizations connect their {{site.data.keyword.cloud_notm}}-based SAP/Db2 environment to on-premises systems and applications. A single network link between the cloud and the enterprise networks is a potential point of failure that needs protection too.

The following diagram shows a highly available network connection between the enterprise and {{site.data.keyword.cloud_notm}}:

![Highly available networking between an enterprise and {{site.data.keyword.cloud_notm}}](/images/sap-db2-network-ha.drawio.svg "Highly available networking between an enterprise and {{site.data.keyword.cloud_notm}}"){: caption="Highly available networking between an enterprise and {{site.data.keyword.cloud_notm}}" caption-side="bottom"}

In this example, network traffic between the on-premises environment and {{site.data.keyword.cloud_notm}} is through an IPsec tunnel. Separate links exist between two on-premises firewalls and two cloud-based firewalls. The two links protect against a single link or firewall failure. To automate failover between the two network links:

* Configure Generic Routing Encapsulation (GRE) tunnels between the transit gateway and the firewalls, with one tunnel connecting to the first firewall and the other to the second firewall.

* Set up Border Gateway Protocol (eBGP) on the firewalls to advertise the on-premises CIDR to the transit gateway.

* Configure route prepending on the second GRE tunnel to ensure that traffic consistently uses the primary tunnel.

* Enable Bidirectional Forwarding Detection (BFD) on both firewalls, including those on {{site.data.keyword.cloud_notm}} and the on-premises sides.

* If the IPsec tunnel between the on-premises network and {{site.data.keyword.cloud_notm}} fails, route advertisements are automatically removed from the primary GRE tunnel, and traffic will seamlessly switch to the secondary tunnel.

This configuration eliminates dependency on VPC routes, avoiding the need for manual route updates.
