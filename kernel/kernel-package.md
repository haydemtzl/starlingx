# Kernel Package

> linux_version.orig.tar.xz
> Due to licensing restrictions, unclear license information, or failure to comply with the Debian Free Software Guidelines (DFSG), parts of the kernel are removed in order to distribute the source in the main section of the Debian archive.
> The guidelines for firmware removal were set by the Handling source-less firmware in the Linux kernel General Resolution and the position statement by the release managers.

- [Debian Linux Kernel Handbook](https://kernel-team.pages.debian.net/kernel-handbook/)

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j `getconf _NPROCESSORS_ONLN` deb-pkg LOCALVERSION=-custom
```

```sh
user@workstation:~/starlingx/kernel/linux-4.19.0$ cp /boot/config-4.15.0-45-generic .config
user@workstation:~/starlingx/kernel/linux-4.19.0$ make -j `getconf _NPROCESSORS_ONLN` deb-pkg
  HOSTCC  scripts/basic/fixdep
  HOSTCC  scripts/kconfig/conf.o
  YACC    scripts/kconfig/zconf.tab.c
  LEX     scripts/kconfig/zconf.lex.c
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
scripts/kconfig/conf  --syncconfig Kconfig
.config:894:warning: symbol value 'm' invalid for HOTPLUG_PCI_SHPC
.config:1148:warning: symbol value 'm' invalid for NF_NAT_REDIRECT
.config:1151:warning: symbol value 'm' invalid for NF_TABLES_INET
.config:1152:warning: symbol value 'm' invalid for NF_TABLES_NETDEV
.config:1335:warning: symbol value 'm' invalid for NF_TABLES_IPV4
.config:1340:warning: symbol value 'm' invalid for NF_TABLES_ARP
.config:1347:warning: symbol value 'm' invalid for NF_NAT_MASQUERADE_IPV4
.config:1382:warning: symbol value 'm' invalid for NF_TABLES_IPV6
.config:1394:warning: symbol value 'm' invalid for NF_NAT_MASQUERADE_IPV6
.config:1420:warning: symbol value 'm' invalid for NF_TABLES_BRIDGE
.config:3998:warning: symbol value 'm' invalid for HW_RANDOM_TPM
.config:4948:warning: symbol value 'm' invalid for LIRC
.config:6174:warning: symbol value 'm' invalid for SND_SOC_INTEL_SST_TOPLEVEL
.config:6179:warning: symbol value 'm' invalid for SND_SOC_INTEL_MACH
*
* Restart config...
*
*
* General setup
*
Compile also drivers which will not load (COMPILE_TEST) [N/y/?] n
...
...

  UPD     include/config/kernel.release
make clean
/bin/bash ./scripts/package/mkdebian
  TAR     linux-4.19.0+.tar.gz
origversion=$(dpkg-parsechangelog -SVersion |sed 's/-[^-]*$//');\
	mv linux-4.19.0+.tar.gz ../linux-4.19.0+_${origversion}.orig.tar.gz
dpkg-buildpackage -r"fakeroot -u" -a$(cat debian/arch) -i.git -us -uc
dpkg-buildpackage: source package linux-4.19.0+
dpkg-buildpackage: source version 4.19.0+-1
dpkg-buildpackage: source distribution xenial
dpkg-buildpackage: source changed by user <user@workstation>
dpkg-buildpackage: host architecture amd64
 dpkg-source -i.git --before-build linux-4.19.0
 fakeroot -u debian/rules clean
rm -rf debian/*tmp debian/files
make clean
 dpkg-source -i.git -b linux-4.19.0
dpkg-source: warning: no source format specified in debian/source/format, see dpkg-source(1)
dpkg-source: info: using source format '1.0'
dpkg-source: warning: source directory 'linux-4.19.0' is not <sourcepackage>-<upstreamversion> 'linux-4.19.0+-4.19.0+'
dpkg-source: warning: .orig directory name linux-4.19.0.orig is not <package>-<upstreamversion> (wanted linux-4.19.0+-4.19.0+.orig)
dpkg-source: info: building linux-4.19.0+ using existing linux-4.19.0+_4.19.0+.orig.tar.gz
dpkg-source: info: building linux-4.19.0+ in linux-4.19.0+_4.19.0+-1.diff.gz
dpkg-source: warning: ignoring deletion of file .scmversion, use --include-removal to override
dpkg-source: warning: the diff modifies the following upstream files: 
 .clang-format
 .cocciconfig
 .config.old
 .get_maintainer.ignore
 .mailmap
 CREDITS
 LICENSES/exceptions/Linux-syscall-note
 LICENSES/other/Apache-2.0
 LICENSES/other/CDDL-1.0
 LICENSES/other/GPL-1.0
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
 README
dpkg-source: info: use the '3.0 (quilt)' format to have separate and documented changes to upstream files, see dpkg-source(1)
dpkg-source: info: building linux-4.19.0+ in linux-4.19.0+_4.19.0+-1.dsc
dpkg-source: warning: missing information for output field Standards-Version
 debian/rules build
make KERNELRELEASE=4.19.0+ ARCH=x86 KBUILD_SRC=
  HYPERCALLS arch/x86/include/generated/asm/xen-hypercalls.h
  HOSTCC  scripts/basic/fixdep
  DESCEND  objtool
  HOSTCC   /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/fixdep.o
  HOSTLD   /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/fixdep-in.o
  LINK     /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/fixdep
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/exec-cmd.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/help.o
  GEN      /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/arch/x86/lib/inat-tables.c
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/arch/x86/decode.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/pager.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/parse-options.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/run-command.o
  LD       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/arch/x86/objtool-in.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/builtin-check.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/sigchain.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/builtin-orc.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/check.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/orc_gen.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/subcmd-config.o
  LD       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/libsubcmd-in.o
  AR       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/libsubcmd.a
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/orc_dump.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/elf.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/special.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/objtool.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/libstring.o
  CC       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/str_error_r.o
  LD       /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/objtool-in.o
  LINK     /home/user/starlingx/kernel/linux-4.19.0/tools/objtool/objtool
  HOSTCC  arch/x86/tools/relocs_32.o
  HOSTCC  arch/x86/tools/relocs_64.o
  UPD     include/generated/utsrelease.h
  HOSTCC  arch/x86/tools/relocs_common.o
  CC      scripts/mod/empty.o
  HOSTLD  arch/x86/tools/relocs
  HOSTCC  scripts/mod/mk_elfconfig
  CC      scripts/mod/devicetable-offsets.s
  UPD     scripts/mod/devicetable-offsets.h
  MKELF   scripts/mod/elfconfig.h
  HOSTCC  scripts/mod/modpost.o
  HOSTCC  scripts/selinux/mdp/mdp
  HOSTCC  scripts/selinux/genheaders/genheaders
  HOSTCC  scripts/mod/file2alias.o
  HOSTCC  scripts/bin2c
  HOSTCC  scripts/kallsyms
  HOSTCC  scripts/conmakehash
  HOSTCC  scripts/mod/sumversion.o
  HOSTCC  scripts/recordmcount
  CC      kernel/bounds.s
  CC      arch/x86/kernel/asm-offsets.s
  HOSTLD  scripts/mod/modpost
  HOSTCC  scripts/sortextable
  ...
  ...
```
