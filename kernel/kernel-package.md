# Kernel Package

> linux_version.orig.tar.xz
> Due to licensing restrictions, unclear license information, or failure to comply with the Debian Free Software Guidelines (DFSG), parts of the kernel are removed in order to distribute the source in the main section of the Debian archive.
> The guidelines for firmware removal were set by the Handling source-less firmware in the Linux kernel General Resolution and the position statement by the release managers.

- [Debian Linux Kernel Handbook](https://kernel-team.pages.debian.net/kernel-handbook/)

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j `getconf _NPROCESSORS_ONLN` deb-pkg LOCALVERSION=-custom
```

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j `getconf _NPROCESSORS_ONLN` deb-pkg LOCALVERSION=-custom
make clean
/bin/bash ./scripts/package/mkdebian
  TAR     linux-5.0.0-rc6-rt1-custom.tar.gz
origversion=$(dpkg-parsechangelog -SVersion |sed 's/-[^-]*$//');\
	mv linux-5.0.0-rc6-rt1-custom.tar.gz ../linux-5.0.0-rc6-rt1-custom_${origversion}.orig.tar.gz
dpkg-buildpackage -r"fakeroot -u" -a$(cat debian/arch) -i.git -us -uc
dpkg-buildpackage: source package linux-5.0.0-rc6-rt1-custom
dpkg-buildpackage: source version 5.0.0-rc6-rt1-custom-2
dpkg-buildpackage: source distribution xenial
dpkg-buildpackage: source changed by user <user@workstation>
dpkg-buildpackage: host architecture amd64
 dpkg-source -i.git --before-build linux.github
 fakeroot -u debian/rules clean
rm -rf debian/*tmp debian/files
make clean
 dpkg-source -i.git -b linux.github
dpkg-source: warning: no source format specified in debian/source/format, see dpkg-source(1)
dpkg-source: info: using source format '1.0'
dpkg-source: warning: source directory 'linux.github' is not <sourcepackage>-<upstreamversion> 'linux-5.0.0-rc6-rt1-custom-5.0.0-rc6-rt1-custom'
dpkg-source: warning: .orig directory name linux.github.orig is not <package>-<upstreamversion> (wanted linux-5.0.0-rc6-rt1-custom-5.0.0-rc6-rt1-custom.orig)
dpkg-source: info: building linux-5.0.0-rc6-rt1-custom using existing linux-5.0.0-rc6-rt1-custom_5.0.0-rc6-rt1-custom.orig.tar.gz
dpkg-source: info: building linux-5.0.0-rc6-rt1-custom in linux-5.0.0-rc6-rt1-custom_5.0.0-rc6-rt1-custom-2.diff.gz
dpkg-source: error: cannot represent change to vmlinux-gdb.py:
dpkg-source: error:   new version is symlink to /home/user/linux/scripts/gdb/vmlinux-gdb.py
dpkg-source: error:   old version is nonexistent
dpkg-source: warning: ignoring deletion of file .scmversion, use --include-removal to override
dpkg-source: warning: the diff modifies the following upstream files: 
 .clang-format
 .cocciconfig
 .config.old
 .get_maintainer.ignore
 .mailmap
 .version
 0001-StarlingX-Death-of-Arbitrary-Process-Notification.patch
 0002-StarlingX-Affine-Compute-Kernel-Threads.patch
 0002-StarlingX-Kernel-Threads-Compute-CPU-Affinity.patch
 0003-StarlingX-Kernel-Threads-Workqueues-IRQs.patch
 0004-StarlingX-Kernel-Threads-iSCSI.patch
 CREDITS
 LICENSES/exceptions/Linux-syscall-note
 LICENSES/other/Apache-2.0
 LICENSES/other/CDDL-1.0
 LICENSES/other/GPL-1.0
 LICENSES/other/ISC
 LICENSES/other/Linux-OpenIB
 LICENSES/other/MPL-1.1
 LICENSES/other/X11
 LICENSES/preferred/BSD-2-Clause
 LICENSES/preferred/BSD-3-Clause
 LICENSES/preferred/BSD-3-Clause-Clear
 LICENSES/preferred/GPL-2.0
 LICENSES/preferred/LGPL-2.0
 LICENSES/preferred/LGPL-2.1
 LICENSES/preferred/MIT
 MAINTAINERS
 Module.symvers
 README
 patch-4.14-rt1.patch
dpkg-source: info: use the '3.0 (quilt)' format to have separate and documented changes to upstream files, see dpkg-source(1)
dpkg-source: error: unrepresentable changes to source
dpkg-buildpackage: error: dpkg-source -i.git -b linux.github gave error exit status 1
scripts/package/Makefile:70: recipe for target 'deb-pkg' failed
make[1]: *** [deb-pkg] Error 1
Makefile:1390: recipe for target 'deb-pkg' failed
make: *** [deb-pkg] Error 2
```
