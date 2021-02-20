# DTN 101 - Configuring DTN on Google Cloud
This project has been developed by Dr Lara Suzuki and supervised by Vint Cerf, Google Inc.

## Introduction
The ION (interplanetary overlay network) software is a suite of communication protocol implementations designed to support mission operation communications across an end-to-end interplanetary network, which might include on-board (flight) subnets, in-situ planetary or lunar networks, proximity links, deep space links, and terrestrial internets.

## Getting Started with Google Cloud VMs

On [Google Cloud Console](console.cloud.google.com), at the top left, `click` on the hamburger icon. Scrow down until you find `Compute Engine`. Hove over `Compute Engine` and then `click` on `VM Instances`.

<img src="https://github.com/lasuzuki/dtn-gcp/blob/main/blob/img1.png" width=400 align=center>


Click `Create` a new instance. In the new page  add the configurations of the instance:

**Name**: instance-dtn

**Region**: Select the reagion closest to you

**Machine Configuration**: Selec the configuration that will suit your needs. I have selected the default `E2 Series` and **Machine type** `e2-medium (2vCPU, 4 BG memory)`

**Identity and API Access**: Select `Allow full access to all Cloud APIs`. We will be doing a tutorial on using Cloud Vision, so make sure you enable the APIs

**Firewall**: Select Allow HTTP and HTTPS

Hit `Create`. Once the instance is created you will be redirected to the VM Instances page where you can see the `Name` of your instance, the `Zone` where it has been deployed, its `Internal` and `External IPs`.

Once the VM is started you can `SSH` directly into the VM.

## Getting Started with  ION

The latest version of ION (4.0.1) can be downloaded [here](https://sourceforge.net/projects/ion-dtn/files/ion-open-source-4.0.1.tar.gz/download)

On your VM execute the following commands

````
sudo apt update
sudo apt install build-essential -y
sudo apt-get install wget -y
wget https://sourceforge.net/projects/ion-dtn/files/ion-open-source-4.0.1.tar.gz/download
tar xzvf download
````


### Compiling ION using autotools

Follow the standard autoconf method for compiling the project. In the **base ION directory** run:

```
./configure
```
Then compile with:
```
make
````
Finally, install (requires root privileges):
```
sudo make install
```

For Linux based systems, you may need to run `sudo ldconfig` with no arguments after install.

### Programs in ION

The following tools are a few examples of programs availale to you after ION is built:

**1. Daemon and Configuration:**
- `ionadmin` is the administration and configuration interface for the local ION node contacts and manages shared memory resources used by ION.
- `ltpadmin` is the administration and configuration interface for LTP operations on the local ION node.
- `bsspadmin` is the administrative interface for operations of the Bundle Streaming Service Protocol on the local ion node.
- `bpadmin` is the administrative interface for bundle protocol operations on the local ion node.
- `ipnadmin` is the administration and configuration interface for the IPN addressing system and routing on the ION node. (ipn:)
- `killm` is a script which tears down the daemon and any running ducts on a single machine (use ionstop instead).
- `ionstart` is a script which completely configures an ION node with the proper configuration file(s).
- `ionstop` is a script which completely tears down the ION node.
- `ionscript` is a script which aides in the creation and management of configuration files to be used with ionstart.

**2. Simple Sending and Receiving:**
- `bpsource` and `bpsink` are for testing basic connectivity between endpoints. bpsink listens for and then displays messages sent by bpsource.
- `bpsendfile` and `bprecvfile` are used to send files between ION nodes.

**3. Testing and Benchmarking:**
- `bpdriver` benchmarks a connection by sending bundles in two modes: request-response and streaming.
- `bpecho` issues responses to bpdriver in request-response mode.
- `bpcounter` acts as receiver for streaming mode, outputting markers on receipt of data from bpdriver and computing throughput metrics.

**4. Logging:**
- By default, the administrative programs will all trigger the creation of a log file called `ion.log` in the directory where the program is called. This means that write-access in your current working directory is required. The log file itself will contain the expected log information from administrative daemons, but it will also contain error reports from simple applications such as bpsink. 

### The Configuration Files

Below we present the configuration files that you should be aware and configure for ION to execute correctly. 

1. `ionadmin's` configuration file, assigns an identity (node number) to the node, optionally configures the resources that will be made available to the node, and specifies contact bandwidths and one-way transmission times. Specifying the "contact plan" is important in deep-space scenarios where the bandwidth must be managed and where acknowledgments must be timed according to propagation delays. It is also vital to the function of contact-graph routing. [How To](IONconfig_file.md)

2. `ltpadmin's` configuration file, specifies spans, transmission speeds, and resources for the Licklider Transfer Protocol convergence layer. [How To](LTPconfig_file.md)

3. `bpadmin's` configuration file, specifies all of the open endpoints for delivery on your local end and specifies which convergence layer protocol(s) you intend to use. With the exception of LTP, most convergence layer adapters are fully configured in this file. [How To](BPconfig_file.md)

4. `ipnadmin's` configuration file, maps endpoints at "neighboring" (topologically adjacent, directly reachable) nodes to convergence-layer addresses. This file populates the ION analogue to an ARP cache for the "ipn" naming scheme. [How To](IPNconfig_file.md)

5. `ionsecadmin's` configuration file, enables bundle security to avoid error messages in ion.log [How To](IONSECconfig_file.md)

### Testing and Stopping your Connection
Assuming no errors occur with the configuration files above, we are now ready to test loopback communications, and also learn how to properly stop ION nodes. The below items are covered. [How To](Testing_Stopping.md)

1. Testing your connection
2. Stopping the Daemon
3. Creating a single configuration file

### ION File for two nodes using LTP
Assuming no errors occur with the configuration files above, we are now ready to test a two nodes communications, and also learn how to properly stop ION nodes. The single rc file for `host 1` can be found [here](/rcfiles/host1.rc) and for `host 2` [here](/rcfiles/host1.rc).
