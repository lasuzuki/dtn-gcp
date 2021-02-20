# Configuration Files Overview
There are five configuration files about which you should be aware.

1. `ionadmin's` configuration file, assigns an identity (node number) to the node, optionally configures the resources that will be made available to the node, and specifies contact bandwidths and one-way transmission times. Specifying the "contact plan" is important in deep-space scenarios where the bandwidth must be managed and where acknowledgments must be timed according to propagation delays. It is also vital to the function of contact-graph routing.

2. `ltpadmin's` configuration file, specifies spans, transmission speeds, and resources for the Licklider Transfer Protocol convergence layer.

3. `bpadmin's` configuration file, specifies all of the open endpoints for delivery on your local end and specifies which convergence layer protocol(s) you intend to use. With the exception of LTP, most convergence layer adapters are fully configured in this file.

4. `ipnadmin's` configuration file, maps endpoints at "neighboring" (topologically adjacent, directly reachable) nodes to convergence-layer addresses. Our examples use TCP/IP and LTP (over IP/UDP), so it maps endpoint IDs to IP addresses. This file populates the ION analogue to an ARP cache for the "ipn" naming scheme.

5. `ionsecadmin's` configuration file, enables bundle security to avoid error messages in ion.log

## The ION Configuration File

Given to ionadmin either as a file or from the daemon command line, this file configures contacts for the ION node. We will assume that the local node's identification number is `1`.

This file specifies contact times and one-way light times between nodes. This is useful in deep-space scenarios: for instance, Mars may be 20 light-minutes away, or 8. Though only some transport protocols make use of this time (currently, only LTP), it must be specified for all links nonetheless. Times may be relative (prefixed with a + from current time) or absolute. Absolute times, are in the format yyyy/mm/dd-hh:mm:ss. By default, the contact-graph routing engine will make bundle routing decisions based on the contact information provided.

The configuration file lines are as follows:

`1 1 ''`

This command will initialize the ion node to be node number 1.

`1` refers to this being the initialization or `first` command.
`1` specifies the node number of this ion node. (IPN node 1).
`''` specifies the name of a file of configuration commands for the node's use of shared memory and other resources (suitable defaults are applied if you leave this argument as an empty string).
`s`

This will start the ION node. It mostly functions to officially "start" the node in a specific instant; it causes all of ION's protocol-independent background daemons to start running.

`a contact +1 +3600 1 1 100000`

specifies a transmission opportunity for a given time duration between two connected nodes (or, in this case, a loopback transmission opportunity).

`a` adds this entry in the configuration table.
`contac` specifies that this entry defines a transmission opportunity.
`+1` is the start time for the contact (relative to when the s command is issued).
`+3600` is the end time for the contact (relative to when the s command is issued).
`1` is the source node number.
`1` is the destination node number.
`100000` is the maximum rate at which data is expected to be transmitted from the source node to the destination node during this time period (here, it is 100000 bytes / second).

`a range +1 +3600 1 1 1`

specifies a distance between nodes, expressed as a number of light seconds, where each element has the following meaning:

`a` adds this entry in the configuration table.
`range` declares that what follows is a distance between two nodes.
`+1` is the earliest time at which this is expected to be the distance between these two nodes (relative to the time s was issued).
`+3600` is the latest time at which this is still expected to be the distance between these two nodes (relative to the time s was issued).
`1` is one of the two nodes in question.
`1` is the other node.
`1` is the distance between the nodes, measured in light seconds, also sometimes called the "one-way light time" (here, one light second is the expected distance).

`m production 1000000` 

specifies the maximum rate at which data will be produced by the node.

`m` specifies that this is a management command.
`production` declares that this command declares the maximum rate of data production at this ION node.
`1000000` specifies that at most 1000000 bytes/second will be produced by this node.

`m consumption 1000000`

specifies the maximum rate at which data can be consumed by the node.

`m` specifies that this is a management command.
`consumption` declares that this command declares the maximum rate of data consumption at this ION node.
`1000000` specifies that at most 1000000 bytes/second will be consumed by this node.

This will make a final configuration file host1.ionrc which looks like this:

```1 1 ''
s
a contact +1 +3600 1 1 100000
a range +1 +3600 1 1 1
m production 1000000
m consumption 1000000```

