# Kernel Package

> linux_version.orig.tar.xz
> Due to licensing restrictions, unclear license information, or failure to comply with the Debian Free Software Guidelines (DFSG), parts of the kernel are removed in order to distribute the source in the main section of the Debian archive.
> The guidelines for firmware removal were set by the Handling source-less firmware in the Linux kernel General Resolution and the position statement by the release managers.

- [Debian Linux Kernel Handbook](https://kernel-team.pages.debian.net/kernel-handbook/)

```sh
user@workstation:~/starlingx/kernel/linux.github$ make -j `getconf _NPROCESSORS_ONLN` deb-pkg LOCALVERSION=-custom
```
