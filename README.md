# DTN 101 - Configuring DTN on Google Cloud
Copyrights Larissa Suzuki and Vint Cerf, Google Inc

## Introduction
The ION (interplanetary overlay network) software is a suite of communication protocol implementations designed to support mission operation communications across an end-to-end interplanetary network, which might include on-board (flight) subnets, in-situ planetary or lunar networks, proximity links, deep space links, and terrestrial internets.

## Installing ION
The latest version of ION (4.0.1) can be downloaded [here](https://sourceforge.net/projects/ion-dtn/files/ion-open-source-4.0.1.tar.gz/download)

## Compiling ION using autotools
Follow the standard autoconf method for compiling the project. In the base ION directory run:

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
Autotools will usually install packages such as this in the /usr/local/ directory tree. This is where the example configuration files and a copy of this tutorial will end up (that is, in the share/ion subdirectory).

For Linux based systems, you may need to run ldconfig with no arguments after install.

## Quick Start Guide

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
- by default, the administrative programs will all trigger the creation of a log file called `ion.log` in the directory where the program is called. This means that write-access in your current working directory is required. The log file itself will contain the expected log information from administrative daemons, but it will also contain error reports from simple applications such as bpsink. 
