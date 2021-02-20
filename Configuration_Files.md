# Configuration Files Overview
There are five configuration files about which you should be aware.

The first, ionadmin's configuration file, assigns an identity (node number) to the node, optionally configures the resources that will be made available to the node, and specifies contact bandwidths and one-way transmission times. Specifying the "contact plan" is important in deep-space scenarios where the bandwidth must be managed and where acknowledgments must be timed according to propagation delays. It is also vital to the function of contact-graph routing.

The second, ltpadmin's configuration file, specifies spans, transmission speeds, and resources for the Licklider Transfer Protocol convergence layer.

The third, bpadmin's configuration file, specifies all of the open endpoints for delivery on your local end and specifies which convergence layer protocol(s) you intend to use. With the exception of LTP, most convergence layer adapters are fully configured in this file.

The fourth, ipnadmin's configuration file, maps endpoints at "neighboring" (topologically adjacent, directly reachable) nodes to convergence-layer addresses. Our examples use TCP/IP and LTP (over IP/UDP), so it maps endpoint IDs to IP addresses. This file populates the ION analogue to an ARP cache for the "ipn" naming scheme.
