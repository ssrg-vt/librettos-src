# LibrettOS's Source Code

LibrettOS's entire source code spans multiple repositories.
To fetch all child repositories, please execute
```
git submodule update --init
```

PLEASE NOTE THAT THIS IS AN EXPERIMENTAL PROTOTYPE SYSTEM WHICH IS NOT
DESIGNED FOR NORMAL DAILY USE. DO NOT USE IT FOR ANY CRITICAL WORK AND DO
NOT ACCESS/MODIFY CRITICAL DATA. THE CODE IS PROVIDED "AS IS" WITHOUT
ANY GUARANTEES, WE ARE NOT RESPONSIBLE/LIABLE FOR ANY DAMAGE AND/OR DATA LOSS!

Below, we provide a brief description. See each repository for additional
instructions. We will also put a more detailed description here shortly.

* rumprun-smp is a fork of [rumprun](https://github.com/rumpkernel/rumprun)
with SMP and Xen HVM support as well as additional drivers
(e.g., NVMe and ixgbe).

Use gcc 8 or 9 workarounds as described in rumprun-smp's README.

* librettos-network contains the network server implementation.

librettos-network is based on rumprun-smp and contains the following compilation
modes: backend, -b (the network server itself), frontend, -f (the application
side), and a direct mode application (default). The network server also
supports dynamic mode switching (should be used automatically if a device
is granted to the _frontend_ part through PCI passthrough).

For the ixgbe driver, you need to apply patches/ixgbe\_nomsix.patch on top of src-netbsd in librettos-network due to a bug in the driver.

librettos-network contains example scripts in the _examples_ subdirectory.

Use gcc 8 or 9 workarounds as described in rumprun-smp's README.

* librettos-packages contains applications that were tested with and adopted for
LibrettOS.

* xen contains a modified Xen hypervisor.

The modified hypervisor should be installed in lieu of the default one.
It contains support for LibrettOS's network server.
Make sure to fully delete the old one (including headers). To install build
dependencies, your distribution may support corresponding commands such as

```
apt-get install build-essential python-dev python3-dev flex bison libpci-dev
apt-get build-dep xen
```

(For build-dep, you also need to uncomment corresponding deb-src
lines in /etc/apt/sources.list and run 'apt-get update' afterwards'.)

Xen compilation can also fail due to weird path errors. Consider using
'./configure --prefix=/usr' instead of plain './configure'.
Keep in mind that it may completely mess up your system, so please have
a proper backup! Please also make sure you do not have any other Xen
installation in your system.

**Note**: The initial version of the modified Xen hypervisor (4.10.1) was
tested on Ubuntu 17.10. However, that version only worked with gcc 7.
Thus, we transitioned to a newer (4.14.0) version, which we compiled
successfully on Ubuntu 20.04.1 (gcc 9). Switch to the original (librettos)
branch in xen if you wish to use the older version.

* patches also include additional patches, such as the kqueue extension
for NetBSD, 4K mbufs, and the adaptive interrupt moderation (aim) patches.
