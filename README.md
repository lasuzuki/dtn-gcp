# DTN 101 - Running the Interplanetary Internet on Google Cloud :rocket:
This project has been developed by Dr Lara Suzuki :woman_technologist: [![Twitter](https://img.shields.io/twitter/url/https/twitter.com/larasuzuki.svg?style=social&label=Follow%20%40larasuzuki)](https://twitter.com/larasuzuki) and supervised by Vint Cerf :technologist: [![Twitter](https://img.shields.io/twitter/url/https/twitter.com/vgcerf.svg?style=social&label=Follow%20%40vgcerf)](https://twitter.com/vgcerf), both at Google Inc.

[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/lasuzuki/StrapDown.js/graphs/commit-activity)
[![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)](https://lbesson.mit-license.org/)
![Profile views](https://gpvc.arturio.dev/lasuzuki)
[![GitHub contributors](https://img.shields.io/github/contributors/Naereen/StrapDown.js.svg)](https://GitHub.com/lasuzuki/StrapDown.js/graphs/contributors/)
[![Open Source Love svg1](https://badges.frapsoft.com/os/v1/open-source.svg?v=103)](https://github.com/ellerbrock/open-source-badges/)
[![saythanks](https://img.shields.io/badge/say-thanks-ff69b4.svg)](https://saythanks.io/to/lasuzuki)

[![ForTheBadge built-with-science](http://ForTheBadge.com/images/badges/built-with-science.svg)](https://GitHub.com/lasuzuki/)

## Introduction

In this project we demonstrate how to run DTN on Google Cloud using NASA's implementation of the bundle protocol - ION.

The ION (interplanetary overlay network) software is a suite of communication protocol implementations designed to support mission operation communications across an end-to-end interplanetary network, which might include on-board (flight) subnets, in-situ planetary or lunar networks, proximity links, deep space links, and terrestrial internets.

## Getting Started with Google Cloud VMs

On [Google Cloud Console](console.cloud.google.com), at the top left corner, `click` on the hamburger icon. Scrow down until you find `Compute Engine`. Hove over `Compute Engine` and then `click` on `VM Instances`.

<img src="https://github.com/lasuzuki/dtn-gcp/blob/main/blob/img1.png" width=400 align=center>


When prompted, click `Create` a new instance. In the new page, add the configurations of the instance:

1. **Name**: instance-dtn

2. **Region**: Select the reagion closest to you

3. **Machine Configuration**: Selec the configuration that will suit your needs. I have selected the default `E2 Series` and **Machine type** `e2-medium (2vCPU, 4 BG memory)`

4. **Boot Disk**: Debian GNU/Linux 10 (buster)

5. **Identity and API Access**: Select `Allow full access to all Cloud APIs`. We will be doing a tutorial on using Cloud Vision, so make sure you enable the APIs

6. **Firewall**: Select Allow HTTP and HTTPS

Hit `Create`. Once the instance is created you will be redirected to the VM Instances page where you can see the `Name` of your instance, the `Zone` where it has been deployed, and both its `Internal` and `External IPs`.

Once the VM is started you can `SSH` directly into the VM.

## SSH in the Google Cloud VM Instance

Mac and Linux support SSH connection natively. You just need to generate an SSH key pair (public key/private key) to connect securely to the virtual machine.

To generate the SSH key pair to connect securely to the virtual machine, follow these steps:

1. Enter the following command in Terminal: `ssh-keygen -t rsa .` 
2. It will start the key generation process. 
3. You will be prompted to choose the location to store the SSH key pair. 
4. Press `ENTER` to accept the default location
5. Now run the following command: `cat ~/.ssh/id_rsa.pub .` 
6. It will display the public key in the terminal. 
7. Highlight and copy this key

Back in the Google Cloud Console:
1. Click `Compute Engine`
2. Then on `VM Instances`
3. Click on the name of the VM instance you would like to SSH into. 
4. In the new page, at the top toolbar, click `Edit`. 

<img src="https://github.com/lasuzuki/dtn-gcp/blob/main/blob/img2.png" width=400 align=center>

5. At the bottom of the page, locate the `SSH Keys` section. 
6. Click on `Add Item`. 
7. In the `Enter public key` text field, paste your SSH key 
8. Hit `Save`

Now you can just open your terminal on your Mac or Linux machine and type `ssh IP.IP.IP.IP` and you will be on the VM (IP.IP.IP.IP is the external IP of the VM).


## Getting Started with  ION

The latest version of ION (4.0.1) can be downloaded [here](https://sourceforge.net/projects/ion-dtn/files/ion-open-source-4.0.1.tar.gz/download). ION 4.0.1 uses the [version 7 of the Bundle Protocol](https://tools.ietf.org/html/draft-ietf-dtn-bpbis-31).

On your VM execute the following commands

````
$ sudo apt update
$ sudo apt install build-essential -y
$ sudo apt-get install wget -y
$ wget https://sourceforge.net/projects/ion-dtn/files/ion-open-source-4.0.1.tar.gz/download
$ tar xzvf download
````


### Compiling ION using autotools

Follow the standard autoconf method for compiling the project. In the **base ION directory** run:

```
$ ./configure
```
Then compile with:
```
$ make
````
Finally, install (requires root privileges):
```
$ sudo make install
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

1. `ionadmin's` configuration file, assigns an identity (node number) to the node, optionally configures the resources that will be made available to the node, and specifies contact bandwidths and one-way transmission times. Specifying the "contact plan" is important in deep-space scenarios where the bandwidth must be managed and where acknowledgments must be timed according to propagation delays. It is also vital to the function of contact-graph routing - [How To](IONconfig_file.md)

2. `ltpadmin's` configuration file, specifies spans, transmission speeds, and resources for the Licklider Transfer Protocol convergence layer - [How To](LTPconfig_file.md)

3. `bpadmin's` configuration file, specifies all of the open endpoints for delivery on your local end and specifies which convergence layer protocol(s) you intend to use. With the exception of LTP, most convergence layer adapters are fully configured in this file - [How To](BPconfig_file.md)

4. `ipnadmin's` configuration file, maps endpoints at "neighboring" (topologically adjacent, directly reachable) nodes to convergence-layer addresses. This file populates the ION analogue to an ARP cache for the "ipn" naming scheme - [How To](IPNconfig_file.md)

5. `ionsecadmin's` configuration file, enables bundle security to avoid error messages in ion.log - [How To](IONSECconfig_file.md)

### Testing and Stopping your Connection
Assuming no errors occur with the configuration files above, we are now ready to test loopback communications, and also learn how to properly stop ION nodes. The below items are covered in this [How To](Testing_Stopping.md) page.

1. Testing your connection
2. Stopping the Daemon
3. Creating a single configuration file

## ION File for two nodes using LTP
Assuming no errors occur with the configuration files above, we are now ready to test a two nodes communications, and also learn how to properly stop ION nodes. The single rc file for `host 1` can be found [here](/rcfiles/host1.rc) and for `host 2` [here](/rcfiles/host1.rc).

The execution of the two hosts should be performed using the command

````
$ ionstart -I <hostname>.rc
````

The image below illustrates the communication between the two hosts using `bpsink` and `bpsource`.

<img src="https://github.com/lasuzuki/dtn-gcp/blob/main/blob/img6.png" width=600 align=center>

To stop ION in both VM instances, use the command 
```
$ ionstop
```
