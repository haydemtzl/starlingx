# Steps

0. Prerequisites
1. Mainline Kernel Git Tree v4.14 Clone 
2. Mainline Kernel Git Tree v4.14 Compilation Default defconfig
3. Mainline Kernel Git Tree v4.14 Compilation StarlingX defconfig

# Prerequisites

```sh
$ sudo apt-get install git build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache
```

# Mainline Kernel Git Tree Clone v4.14

```sh
$ git clone https://github.com/xe1gyq/linux.git
```


```sh
user@workstation:~/starlingx/kernel/linux.github$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ git checkout -b v4.14 v4.14
Checking out files: 100% (42539/42539), done.
Switched to a new branch 'v4.14'
```

# Mainline Kernel Git Tree v4.14 Compilation Default defconfig

## [PATCH] Notification of death of arbitrary processes

### CONFIG_SIGEXIT=n

```sh
user@workstation:~/starlingx/kernel/linux.github$ make oldconfig
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j5
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make menuconfig
```

### CONFIG_SIGEXIT=y

```
    General setup  --->
[*] Notification of death of arbitrary processes
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j5
```

# Mainline Kernel Git Tree v4.14 Compilation StarlingX defconfig

```sh
$ cp /localdisk/loadbuild/builder/starlingx/std/results/builder-starlingx-tis-r5-pike-std/kernel-3.10.0-957.1.3.el7.1.tis/kernel-3.10.0-957.1.3.el7.1.tis.src.rpm .
```

```sh
$ rpm2cpio ./kernel-3.10.0-957.1.3.el7.1.tis.src.rpm | cpio -idmv
```

```sh
[builder@3327e4ee19d7 kernel-3.10.0-957.1.3.el7.1.tis.src]$ ls
affine-compute-kernel-threads.patch                         kernel-3.10.0-s390x.config
Affine-irqs-and-workqueues-with-kthread_cpus.patch          kernel-3.10.0-s390x-debug.config
aic94xx-Skip-reading-user-settings-if-flash-is-not-f.patch  kernel-3.10.0-s390x-kdump.config
centos-ca-secureboot.der                                    kernel-3.10.0-x86_64.config
centos-kpatch.x509                                          kernel-3.10.0-x86_64.config.tis_extra
centos-ldup.x509                                            kernel-3.10.0-x86_64-debug.config
centossecureboot001.crt                                     kernel-abi-whitelists-957.tar.bz2
CGTS-3744-route-do-not-cache-fib-route-info-on-local.patch  kernel-kabi-dw-957.tar.bz2
check-kabi                                                  kernel.spec
cma-add-placement-specifier-for-cma-kernel-parameter.patch  linux-3.10.0-957.1.3.el7.tar.xz
cpuidle-menu-add-per-CPU-PM-QoS-resume-latency-consi.patch  linux-kernel-test.patch
cpuidle-menu-Avoid-taking-spinlock-for-accessing-QoS.patch  Makefile.common
cpuidle-menu-stop-seeking-deeper-idle-if-current-sta.patch  Make-kernel-start-eth-devices-at-offset.patch
CPU-PM-expose-pm_qos_resume_latency-for-CPUs.patch          memblock-introduce-memblock_alloc_range.patch
cpupower.config                                             modprobe-dccp-blacklist.conf
cpupower.service                                            Module.kabi_dup_ppc64
debrand-rh-i686-cpu.patch                                   Module.kabi_dup_ppc64le
debrand-rh_taint.patch                                      Module.kabi_dup_s390x
debrand-single-cpu.patch                                    Module.kabi_dup_x86_64
dpt_i2o-fix-build-warning.patch                             Module.kabi_ppc64
Enable-building-kernel-with-CONFIG_BLK_DEV_NBD.patch        Module.kabi_ppc64le
Enable-building-mpt2sas-and-mpt3sas-as-builtin-for-C.patch  Module.kabi_s390x
extra_certificates                                          Module.kabi_x86_64
Fix-cacheinfo-compilation-issues-for-3.10.patch             Notification-of-death-of-arbitrary-processes.patch
fix-CentOS-7.6-upgrade-compile-error.patch                  PCI-Add-ACS-quirk-for-Intel-Fortville-NICs.patch
fix-compilation-issues.patch                                Porting-Cacheinfo-from-Kernel-4.10.17.patch
Fix-compile-issue-when-transparent-hugepages-are-off.patch  rcu-Don-t-wake-rcuc-X-kthreads-on-NOCB-CPUs.patch
ima_signing_key.pub                                         sign-modules
intel-iommu-allow-ignoring-Ethernet-device-RMRR-with.patch  turn-off-write-same-in-smartqpi-driver.patch
kernel                                                      US101216-IMA-support-in-Titanium-kernel.patch
kernel-3.10.0-957.1.3.el7.1.tis.src.rpm                     US103091-IMA-System-Configuration.patch
kernel-3.10.0-ppc64.config                                  x509.genkey
kernel-3.10.0-ppc64-debug.config                            x86-enable-DMA-CMA-with-swiotlb.patch
kernel-3.10.0-ppc64le.config                                x86-make-dma_alloc_coherent-return-zeroed-memory-if-.patch
kernel-3.10.0-ppc64le-debug.config
```

- kernel-3.10.0-x86_64.config.tis_extra
- linux-3.10.0-957.1.3.el7.tar.xz

```sh
$ cp /home/user/starlingx/workspace/localdisk/designer/builder/starlingx/sandbox/kernel-3.10.0-957.1.3.el7.1.tis.src/kernel-3.10.0-x86_64.config.tis_extra ~/starlingx/kernel/linux.github/
```