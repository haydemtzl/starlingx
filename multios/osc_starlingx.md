# openSUSE:Open Build Service Command Line Tool StarlingX

## home:xe1gyq > platform-kickstarts

Expected Output:
- - https://build.opensuse.org/package/show/home:xe1gyq/platform-kickstarts

### Web Service

1. Under your workspace (e.g. https://build.opensuse.org/project/show/home:xe1gyq), select "Create Package"
2. Fill out and Accept:
   - Name: platform-kickstarts
   - Title: Platform kickstart files
   - Description: Platform kickstart files
3. Select "Repositories" column
4. Select "Add repositories" link
5. Select a repository (e.g. SLE_12_SP4 (x86_64) SUSE:SLE-12-SP4:GA/standard)

### Container Setup

```
user@workstation:~$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc-xe1gyq jaltek/docker-opensuse-osc-client /bin/bash
:/ # 
```

### Common Package Installation

```sh
:/ # zypper install vim curl
```

### Package Checkout

```sh
:/ # osc co home:xe1gyq platform-kickstarts

Your user account / password are not configured yet.
You will be asked for them below, and they will be stored in
/root/.oscrc for future use.

Creating osc configuration file /root/.oscrc ...
Username: xe1gyq
Password: 
done
A    home:xe1gyq
A    home:xe1gyq/platform-kickstarts
At revision None.
```

```sh
:/ # cd home:xe1gyq/platform-kickstarts/
:/home:xe1gyq/platform-kickstarts # 
```

### Base specfile creation

```sh
:/home:xe1gyq/platform-kickstarts # vi platform-kickstarts.spec
```

```sh
Name:           platform-kickstarts
Version:        1.0
Release:        %{tis_patch_ver}%{?tis_dist}
Summary:        Placeholder for platform-kickstarts
License:        Apache-2.0

Source0:        %{name}-%{version}.tar.gz

%description

%files

%changelog
```

```sh
:/home:xe1gyq/platform-kickstarts # osc meta prj -e home:xe1gyq
```

```sh
<project name="home:xe1gyq">
  <title/>
  <description/>
  <person userid="xe1gyq" role="maintainer"/>
  <repository name="platform-kickstarts">
    <path project="SUSE:SLE-12-SP4:GA" repository="standard"/>
    <arch>x86_64</arch>
  </repository>
</project>
```

```sh
Sending meta data...
Done.
```

```sh
:/home:xe1gyq # osc meta pkg -e home:xe1gyq platform-kickstarts
```

```sh
<package name="platform-kickstarts" project="home:xe1gyq">
  <title>Platform kickstart files</title>
  <description>Platform kickstart files</description>
</package>
```

```sh
:/home:xe1gyq/platform-kickstarts # osc rebuildpac home:xe1gyq platform-kickstarts
ok
```

```
:/home:xe1gyq/platform-kickstarts # osc add *
A    platform-kickstarts.spec
:/home:xe1gyq/platform-kickstarts # osc commit
Sending    platform-kickstarts.spec
Transmitting file data .
Committed revision 1.
```

```sh
:/home:xe1gyq/platform-kickstarts # osc up
At revision 1.
```

```sh
:/home:xe1gyq/platform-kickstarts # osc rebuildpac home:xe1gyq platform-kickstarts      
ok
```

```sh
:/home:xe1gyq/platform-kickstarts # osc build --no-verify platform-kickstarts x86_64 platform-kickstarts.spec 
Building platform-kickstarts.spec for platform-kickstarts/x86_64
Getting buildinfo from server and store to /home:xe1gyq/platform-kickstarts/.osc/_buildinfo-platform-kickstarts-x86_64.xml
Getting buildconfig from server and store to /home:xe1gyq/platform-kickstarts/.osc/_buildconfig-platform-kickstarts-x86_64
...
... # Taking ~ 10 minutes
...
[    6s] -----------------------------------------------------------------
[    6s] ----- building platform-kickstarts.spec (user abuild)
[    6s] -----------------------------------------------------------------
[    6s] -----------------------------------------------------------------
[    6s] + exec rpmbuild -ba --define '_srcdefattr (-,root,root)' --nosignature /home/abuild/rpmbuild/SOURCES/platform-kickstarts.spec
[    6s] error: Bad file: /home/abuild/rpmbuild/SOURCES/platform-kickstarts-1.0.tar.gz: No such file or directory
[    6s] 
[    6s] 
[    6s] RPM build errors:
[    6s]     Bad file: /home/abuild/rpmbuild/SOURCES/platform-kickstarts-1.0.tar.gz: No such file or directory

The buildroot was: /var/tmp/build-root/platform-kickstarts-x86_64
```

```sh
:/home:xe1gyq/platform-kickstarts # osc add platform-kickstarts.spec
osc: warning: 'platform-kickstarts.spec' is already under version control
:/home:xe1gyq/platform-kickstarts # osc commit
Sending    platform-kickstarts.spec
Transmitting file data .
Committed revision 2.
```

### Metal Tar Gz File

```sh
:/home:xe1gyq/platform-kickstarts # wget https://opendev.org/starlingx/metal/archive/master.tar.gz
```

```sh
:/home:xe1gyq/platform-kickstarts # tar xvf master.tar.gz 
:/home:xe1gyq/platform-kickstarts # mv metal/ platform-kickstarts-1.0/
:/home:xe1gyq/platform-kickstarts # tar -zcvf platform-kickstarts-1.0.tar.gz platform-kickstarts-1.0/
:/home:xe1gyq/platform-kickstarts # rm -rf master.tar.gz platform-kickstarts-1.0/
```

```sh
:/home:xe1gyq/platform-kickstarts # osc build --no-verify platform-kickstarts x86_64 platform-kickstarts.spec
...
...
[   12s] 2 packages and 0 specfiles checked; 0 errors, 17 warnings.
[   12s] 
[   12s] 
[   12s] 090c02e38358 finished "build platform-kickstarts.spec" at Fri May 17 13:31:54 UTC 2019.
[   12s] 

/var/tmp/build-root/platform-kickstarts-x86_64/home/abuild/rpmbuild/SRPMS/platform-kickstarts-1.0-%{tis_patch_ver}.src.rpm

/var/tmp/build-root/platform-kickstarts-x86_64/home/abuild/rpmbuild/RPMS/x86_64/platform-kickstarts-1.0-%{tis_patch_ver}.x86_64.rpm
```

### Specfile Refactor

- Content from remote
  - https://opendev.org/starlingx/metal/src/branch/master/kickstart/centos/platform-kickstarts.spec
- To local
  - platform-kickstarts.spec

### Local Packaging

```sh
:/home:xe1gyq/platform-kickstarts # osc build --no-verify platform-kickstarts x86_64 platform-kickstarts.spec
...
...
   22s] platform-kickstarts-extracfgs.noarch: W: description-shorter-than-summary
[   22s] platform-kickstarts-pxeboot.noarch: W: description-shorter-than-summary
[   22s] The package description should be longer than the summary. be a bit more
[   22s] verbose, please.
[   22s] 
[   22s] 4 packages and 0 specfiles checked; 0 errors, 42 warnings.
[   22s] 
[   22s] 
[   22s] 090c02e38358 finished "build platform-kickstarts.spec" at Fri May 17 14:40:22 UTC 2019.
[   22s] 

/var/tmp/build-root/platform-kickstarts-x86_64/home/abuild/rpmbuild/SRPMS/platform-kickstarts-1.0-%{tis_patch_ver}.src.rpm

/var/tmp/build-root/platform-kickstarts-x86_64/home/abuild/rpmbuild/RPMS/noarch/platform-kickstarts-1.0-%{tis_patch_ver}.noarch.rpm
/var/tmp/build-root/platform-kickstarts-x86_64/home/abuild/rpmbuild/RPMS/noarch/platform-kickstarts-extracfgs-1.0-%{tis_patch_ver}.noarch.rpm
/var/tmp/build-root/platform-kickstarts-x86_64/home/abuild/rpmbuild/RPMS/noarch/platform-kickstarts-pxeboot-1.0-%{tis_patch_ver}.noarch.rpm
```

### Remote Packaging

```sh
[   28s] ### VM INTERACTION START ###
[   31s] [   23.453180] sysrq: SysRq : Power Off
[   31s] [   23.457807] reboot: Power down
[   31s] ### VM INTERACTION END ###
[   31s] build: extracting built packages...
[   31s] RPMS/noarch/platform-kickstarts-pxeboot-1.0-4.1.noarch.rpm
[   31s] RPMS/noarch/platform-kickstarts-1.0-4.1.noarch.rpm
[   31s] RPMS/noarch/platform-kickstarts-extracfgs-1.0-4.1.noarch.rpm
[   31s] SRPMS/platform-kickstarts-1.0-4.1.src.rpm
[   31s] OTHER/rpmlint.log
[   31s] OTHER/_statistics
```

## home:marcelarosalesj stx-nfv

- https://build.opensuse.org/project/show/Cloud:StarlingX:2.0
- https://build.opensuse.org/project/show/home:marcelarosalesj

### Container

```shuser@workstation:~$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc-xe1gyq jaltek/docker-opensuse-osc-client /bin/bash
:/ # 

user@workstation:~$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc jaltek/docker-opensuse-osc-client /bin/bash
:/ # 
```

```sh
:/ # osc co home:marcelarosalesj stx-nfv 

Your user account / password are not configured yet.
You will be asked for them below, and they will be stored in
/root/.oscrc for future use.

Creating osc configuration file /root/.oscrc ...
Username: xe1gyq
Password: 
done
A    home:marcelarosalesj
A    home:marcelarosalesj/stx-nfv
A    home:marcelarosalesj/stx-nfv/_multibuild
A    home:marcelarosalesj/stx-nfv/nfv.spec
A    home:marcelarosalesj/stx-nfv/nova-api-proxy.spec
A    home:marcelarosalesj/stx-nfv/stx-nfv-1.0.tar.gz
A    home:marcelarosalesj/stx-nfv/stx-nfv.spec
At revision 4.
:/ # 
```

```sh
:/ # cd home:marcelarosalesj/stx-nfv
:/home:marcelarosalesj/stx-nfv # ls
_multibuild  nfv.spec  nova-api-proxy.spec  stx-nfv-1.0.tar.gz  stx-nfv.spec
```

```sh
:/home:marcelarosalesj/stx-nfv # osc build --no-verify stx-nfv.spec  
Building stx-nfv.spec for CentOS_7/x86_64
Getting buildinfo from server and store to /home:marcelarosalesj/stx-nfv/.osc/_buildinfo-CentOS_7-x86_64.xml
Getting buildconfig from server and store to /home:marcelarosalesj/stx-nfv/.osc/_buildconfig-CentOS_7-x86_64
Updating cache of required packages
...
... # Wait for ~ 10 minutes
...
[  285s] Processing files: stx-nfv-1.0-%{tis_patch_ver}.x86_64
[  285s] Provides: stx-nfv = 1.0-%{tis_patch_ver} stx-nfv(x86-64) = 1.0-%{tis_patch_ver}
[  285s] Requires(rpmlib): rpmlib(FileDigests) <= 4.6.0-1 rpmlib(PayloadFilesHavePrefix) <= 4.0-1 rpmlib(CompressedFileNames) <= 3.0.4-1
[  285s] Checking for unpackaged file(s): /usr/lib/rpm/check-files /home/abuild/rpmbuild/BUILDROOT/stx-nfv-1.0-%{tis_patch_ver}.x86_64
[  288s] Wrote: /home/abuild/rpmbuild/SRPMS/stx-nfv-1.0-%{tis_patch_ver}.src.rpm
[  288s] Wrote: /home/abuild/rpmbuild/RPMS/x86_64/stx-nfv-1.0-%{tis_patch_ver}.x86_64.rpm
[  288s] Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.rW8s0l
[  288s] + umask 022
[  288s] + cd /home/abuild/rpmbuild/BUILD
[  288s] + /usr/bin/rm -rf '/home/abuild/rpmbuild/BUILDROOT/stx-nfv-1.0-%{tis_patch_ver}.x86_64'
[  288s] + exit 0
[  288s] ... checking for files with abuild user/group
[  288s] 
[  288s] a5fe44b9314d finished "build stx-nfv.spec" at Fri May 17 08:15:22 UTC 2019.
[  288s] 

/var/tmp/build-root/CentOS_7-x86_64/home/abuild/rpmbuild/SRPMS/stx-nfv-1.0-%{tis_patch_ver}.src.rpm

/var/tmp/build-root/CentOS_7-x86_64/home/abuild/rpmbuild/RPMS/x86_64/stx-nfv-1.0-%{tis_patch_ver}.x86_64.rpm
```
