# IPN Routing Configuration

As noted earlier, this file is used to build ION's analogue to an ARP cache, a table of `egress plans`. It specifies which outducts to use in order to forward bundles to the local node's neighbors in the network. Since we only have one outduct, for forwarding bundles to one place (the local node), we only have one egress plan.

````
a plan 1 ltp/1
````

This command defines an egress plan for bundles to be transmitted to the local node:

`a` means this command adds something.
`plan` means this command adds an egress plan.
`1` is the node number of the remote node. In this case, that is the local node's own node number; we're configuring for loopback.
`ltp/1` is the identifier of the outduct through which to transmit bundles in order to convey them to this ''remote'' node.

This means that the entire configuration file host1.ipnrc looks like this:

````
a plan 1 ltp/1
````
