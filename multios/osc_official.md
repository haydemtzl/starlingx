# Multi-OS

## Strategy

- https://build.opensuse.org/project/show/Cloud:StarlingX:2.0

## Personal

> https://build.opensuse.org/project/show/home:xe1gyq

- mtce-storage
- platform-kickstarts
- sm-tools

## home:xe1gyq > mtce-storage

```sh
$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc-mtce-storage jaltek/docker-opensuse-osc-client /bin/bash
```

```sh
:/ # zypper install vim curl
```

```sh
:/ # osc sr home:xe1gyq mtce-storage Cloud:StarlingX:2.0 mtce-storage

Your user account / password are not configured yet.
You will be asked for them below, and they will be stored in
/root/.oscrc for future use.

Creating osc configuration file /root/.oscrc ...
Username: xe1gyq
Password: 
done
Warning: failed to fetch meta data for 'Cloud:StarlingX:2.0' package 'mtce-storage' (new package?) 
```

```sh
From Story "SUSE Spec Files for stx/metal"
https://storyboard.openstack.org/#!/story/2005684
Task 31076 "SUSE Specfile for mtce-storage"
```

```sh
created request id 706082
```

```sh
:/ # osc co home:xe1gyq mtce-storage
```

```sh
:/ # cd home:xe1gyq/mtce-storage
```

```sh
:/home:xe1gyq/mtce-storage # osc build --no-verify SLE_12_SP4 x86_64 mtce-storage.spec
```

```sh
:/home:xe1gyq/mtce-storage # osc build --no-verify openSUSE_Leap_15.0 x86_64 mtce-storage.spec
```

Error

```sh
[   34s] + rm -rf '/home/abuild/rpmbuild/BUILDROOT/mtce-storage-1.0-%{tis_patch_ver}.x86_64'
[   34s] + exit 0
[   34s] ... checking for files with abuild user/group
[   34s] ... running 00-check-install-rpms
[   34s] ... installing all built rpms
[   34s] error: File not found by glob: /.build.packages/RPMS/x86_64/mtce-storage-1.0-%{tis_patch_ver}.x86_64.rpm
[   34s] failed to install rpms, aborting build

The buildroot was: /var/tmp/build-root/openSUSE_Leap_15.0-x86_64
:/home:xe1gyq/mtce-storage # 
```

## home:xe1gyq > platform-kickstarts

```sh
user@workstation:~$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc-platform-kickstarts jaltek/docker-opensuse-osc-client /bin/bash
```

```sh
:/ # zypper install vim curl
```

```sh
:/ # osc sr home:xe1gyq platform-kickstarts Cloud:StarlingX:2.0 platform-kickstarts
Warning: failed to fetch meta data for 'Cloud:StarlingX:2.0' package 'platform-kickstarts' (new package?) 
```

```sh
From Story "SUSE Spec Files for stx/metal"
https://storyboard.openstack.org/#!/story/2005684
Task 31076 "SUSE Specfile for platform-kickstarts"
```

```sh
created request id 706087
```

```sh
:/ # osc co home:xe1gyq platform-kickstarts
```

```sh
:/ # cd home:xe1gyq/platform-kickstarts
```

```sh
:/home:xe1gyq/mtce-storage # osc build --no-verify SLE_12_SP4 x86_64 platform-kickstarts.spec
```

```sh
:/home:xe1gyq/mtce-storage # osc build --no-verify openSUSE_Leap_15.0 x86_64 platform-kickstarts.spec
```

Error

```sh
[   29s] + exit 0
[   29s] ... checking for files with abuild user/group
[   29s] ... running 00-check-install-rpms
[   29s] ... installing all built rpms
[   29s] error: File not found by glob: /.build.packages/RPMS/noarch/platform-kickstarts-1.0-%{tis_patch_ver}.noarch.rpm
[   29s] error: File not found by glob: /.build.packages/RPMS/noarch/platform-kickstarts-extracfgs-1.0-%{tis_patch_ver}.noarch.rpm
[   29s] error: File not found by glob: /.build.packages/RPMS/noarch/platform-kickstarts-pxeboot-1.0-%{tis_patch_ver}.noarch.rpm
[   29s] failed to install rpms, aborting build

The buildroot was: /var/tmp/build-root/openSUSE_Leap_15.0-x86_64
:/home:xe1gyq/platform-kickstarts # 
:/home:xe1gyq/platform-kickstarts # osc build --no-verify openSUSE_Leap_15.0 x86_64 platform-kickstarts.spec 
```

## home:xe1gyq > sm-tools

```sh
user@workstation:~$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc-sm-tools jaltek/docker-opensuse-osc-client /bin/bash
```

```sh
:/ # zypper install vim curl
```

```sh
:/ # osc sr home:xe1gyq sm-tools Cloud:StarlingX:2.0 sm-tools

Your user account / password are not configured yet.
You will be asked for them below, and they will be stored in
/root/.oscrc for future use.

Creating osc configuration file /root/.oscrc ...
Username: xe1gyq
Password: 
done
Warning: failed to fetch meta data for 'Cloud:StarlingX:2.0' package 'sm-tools' (new package?) 
```

```sh
From Story "SUSE Spec Files for stx/nfv"
https://storyboard.openstack.org/#!/story/2005724
Task 31076 "SUSE Specfile for sm-tools"
```

```sh
created request id 706091
```

```sh
:/ # osc co home:xe1gyq sm-tools
```

```sh
:/ # cd home:xe1gyq/sm-tools
```

```sh
:/home:xe1gyq/sm-tools # osc build --no-verify SLE_12_SP4 x86_64 sm-tools.spec           
```

```sh
:/home:xe1gyq/sm-tools # osc build --no-verify openSUSE_Leap_15.0 x86_64 sm-tools.spec
```
