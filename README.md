# LibrettOS's Source Code

LibrettOS's entire source code spans multiple repositories.
To fetch all child repositories, please execute
```
git submodule update --init
```

Below, we provide a brief description. See each repository for additional
instructions. We will also put a more detailed description here shortly.

* rumprun-smp is a fork of [rumprun](https://github.com/rumpkernel/rumprun)
with SMP and Xen HVM support as well as additional drivers
(e.g., NVMe and ixgbe).

* librettos-network contains the network server implementation.

librettos-network is based on rumprun-smp and contains the following compilation
modes: backend, -b (the network server itself), frontend, -f (the application
side), and a direct mode application (default). The network server also
supports dynamic mode switching (should be used automatically if a device
is granted to the _frontend_ part through PCI passthrough).

For the ixgbe driver, you need to apply patches/ixgbe\_nomsix.patch on top of src-netbsd in librettos-network due to a bug in the driver.

librettos-network contains example scripts in the _examples_ subdirectory.

* librettos-packages contains applications that were tested with and adopted for
LibrettOS.

* xen contains a modified Xen hypervisor.

The modified hypervisor should be installed in lieu of the default one.
It contains support for LibrettOS's network server.
Make sure to fully delete the old one (including headers). To install build
dependencies, your distribution may support corresponding commands such as

```
apt-get install build-essential
apt-get install build-dep xen
```

**Note**: Xen currently does not compile with gcc 8, so you need to use gcc 7.
You can use Ubuntu 17.10 or Ubuntu 18.04 LTS. You may also need to apply
patches/xen-memfd.patch if the Xen compilation fails due to memfd issues.
This failure depends on what Linux distribution you use. (Only apply it if
you see errors, do _not_ do a fresh compilation when so happens.)

* patches also include additional patches, such as the kqueue extension
for NetBSD, 4K mbufs, and the adaptive interrupt moderation (aim) patches.
