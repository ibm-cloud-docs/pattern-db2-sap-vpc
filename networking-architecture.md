---

copyright:
  years: 2025
lastupdated: "2025-03-27"

subcollection: pattern-db2-sap-vpc

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Architecture decisions for networking
{: #networking-architecture}

The following link will take you to the networking architectural decisions for SAP running on IBM Cloud VPC:

[Architecture decisions for networking](/docs/pattern-sap-on-vpc?topic=pattern-sap-on-vpc-network-decisions)


## Resilient communication between enterprise networks and the SAP / Db2 servers can xxxxxx
On-prem to IBM Cloud traffic comes via an IPSec tunnel. xxxxx


* Configure GRE tunnels between the Transit Gateway (TGW) and the firewalls, with one tunnel connecting to Firewall1 and the other to Firewall2.

* Set up eBGP on the Palo Alto firewall to advertise the on-premises CIDR to the TGW.

* Configure route prepending on the second GRE tunnel to ensure traffic consistently uses the primary tunnel.

* Enable BFD on both firewalls, including those on IBM and the on-premises side.

* If the IPSec tunnel between the on-premises network and IBM goes down, route advertisements will automatically be removed from the primary GRE tunnel, and traffic will seamlessly switch to the secondary tunnel.

* This configuration eliminates dependency on VPC routes, avoiding the need for manual route updates.
