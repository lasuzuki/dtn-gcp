# DTN 101 - Configuring DTN on Google Cloud
Copyrights Larissa Suzuki and Vint Cerf, Google Inc

# Introduction
The ION (interplanetary overlay network) software is a suite of communication protocol implementations designed to support mission operation communications across an end-to-end interplanetary network, which might include on-board (flight) subnets, in-situ planetary or lunar networks, proximity links, deep space links, and terrestrial internets.

# Installing ION
The latest version of ION (4.0.1) can be downloaded [here](https://sourceforge.net/projects/ion-dtn/files/ion-open-source-4.0.1.tar.gz/download)

# Compiling ION using autotools
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

