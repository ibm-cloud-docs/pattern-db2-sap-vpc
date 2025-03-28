---

copyright:
  years: 2025
lastupdated: "2025-03-28"

subcollection: pattern-db2-sap-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

The following link will take you to the networking architectural decisions for SAP running on IBM Cloud VPC:

[Architecture decisions for networking](/docs/pattern-sap-on-vpc?topic=pattern-sap-on-vpc-network-decisions)


## Resilient communication between enterprise networks and IBM Cloud servers
{: #resilient-comms-to-cloud}

Most organisations will connect their IBM Cloud-based SAP/Db2 environment to on-premises systems and applications. A single network link between the cloud and the enterprise networks is a potential point of failure so needs protection too.

The following diagram shows a highly available network connection between the enterprise and IBM Cloud:

![Highly available networking between an enterprise and IBM Cloud](/images/sap-db2-network-ha.drawio.svg "Highly available networking between an enterprise and IBM Cloud"){: caption="Highly available networking between an enterprise and IBM Cloud" caption-side="bottom"}

In this example, network traffic between the on-premises environment and IBM Cloud is via an IPSec tunnel. Separate links exist between two on-premises firewalls and two cloud-based firewalls.  The two links protect against a single link or firewall failure. To automate failover between the two network links:

* Configure Generic Routing Encapsulation (GRE) tunnels between the Transit Gateway (TGW) and the firewalls, with one tunnel connecting to the first firewall and the other to the second firewall.

* Set up eBGP (Border Gateway Protocol) on the firewalls to advertise the on-premises CIDR to the Transit Gateway.

* Configure route prepending on the second GRE tunnel to ensure traffic consistently uses the primary tunnel.

* Enable Bidiretional Forwarding Detection (BFD) on both firewalls, including those on IBM Cloud and the on-premises sides.

* If the IPSec tunnel between the on-premises network and IBM Cloud fails, route advertisements will automatically be removed from the primary GRE tunnel, and traffic will seamlessly switch to the secondary tunnel.

This configuration eliminates dependency on VPC routes, avoiding the need for manual route updates.
