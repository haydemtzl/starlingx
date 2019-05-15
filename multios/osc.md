
# openSUSE:Open Build Service Command Line Tool

- https://build.opensuse.org/

## Setup

```sh
906ba71fcce3:/home:xe1gyq # zypper install vim
```

## openSUSE:Build Service Tutorial StarlingX saulwold stx-fault

> Build FLOCK services for another distro, populate with FLOCK content

### Steps

1. Go to https://build.opensuse.org/project/show/home:saulwold
2. Select poackage stx-fault
3. Branch Package
```sh
Source
home:saulwold / stx-fault
Destination
home:xe1gyq:branches:home:saulwold
```
4. https://build.opensuse.org/package/show/home:xe1gyq:branches:home:saulwold/stx-fault should be available
5. Checkout stx-fault from Saul workspace

```sh
906ba71fcce3:~/stx-fault # osc co home:saulwold stx-fault
A    home:saulwold
A    home:saulwold/stx-fault
A    home:saulwold/stx-fault/_multibuild
A    home:saulwold/stx-fault/debian.tar.xz
A    home:saulwold/stx-fault/fm-api.spec
A    home:saulwold/stx-fault/fm-common.dsc
A    home:saulwold/stx-fault/fm-common.spec
A    home:saulwold/stx-fault/fm-doc.spec
A    home:saulwold/stx-fault/fm-mgr.spec
A    home:saulwold/stx-fault/fm-rest-api.spec
A    home:saulwold/stx-fault/python-fmclient.spec
A    home:saulwold/stx-fault/snmp-audittrail.spec
A    home:saulwold/stx-fault/snmp-ext.spec
A    home:saulwold/stx-fault/stx-fault-1.0.tar.xz
A    home:saulwold/stx-fault/stx-fault.spec
At revision 26.
```

What is the content?

```sh
906ba71fcce3:~/stx-fault # ls
home:saulwold
906ba71fcce3:~/stx-fault # ls home:saulwold/
stx-fault
906ba71fcce3:~/stx-fault # ls home:saulwold/stx-fault/
_multibuild    fm-api.spec    fm-common.spec  fm-mgr.spec       python-fmclient.spec  snmp-ext.spec         stx-fault.spec
debian.tar.xz  fm-common.dsc  fm-doc.spec     fm-rest-api.spec  snmp-audittrail.spec  stx-fault-1.0.tar.xz
```

```sh
906ba71fcce3:~/stx-fault # cd home:saulwold/stx-fault/
906ba71fcce3:~/stx-fault/home:saulwold/stx-fault # ls   
_multibuild    fm-api.spec    fm-common.spec  fm-mgr.spec       python-fmclient.spec  snmp-ext.spec         stx-fault.spec
debian.tar.xz  fm-common.dsc  fm-doc.spec     fm-rest-api.spec  snmp-audittrail.spec  stx-fault-1.0.tar.xz
```

Now let's build, first try we have an error:

```sh
906ba71fcce3:~/stx-fault/home:saulwold/stx-fault # osc build --no-verify fm-common.spec
Building fm-common.spec for SLE_12_SP4/x86_64
Getting buildinfo from server and store to /root/stx-fault/home:saulwold/stx-fault/.osc/_buildinfo-SLE_12_SP4-x86_64.xml
...
... # Taking around 10 minutes
...
[  182s] calling /usr/lib/rpm/brp-suse.d/brp-15-strip-debug
[  182s] /usr/lib/rpm/brp-suse.d/brp-15-strip-debug: line 33: /dev/fd/62: No such file or directory
[  182s] /usr/lib/rpm/brp-suse.d/brp-15-strip-debug: line 47: /dev/fd/62: No such file or directory
[  182s] error: Bad exit status from /var/tmp/rpm-tmp.Qtn3Vt (%install)
[  182s] 
[  182s] 
[  182s] RPM build errors:
[  182s]     Bad exit status from /var/tmp/rpm-tmp.Qtn3Vt (%install)

The buildroot was: /var/tmp/build-root/SLE_12_SP4-x86_64
```

Second try? Work in Progress

```sh
906ba71fcce3:~/stx-fault/home:saulwold/stx-fault # pwd
/root/stx-fault/home:saulwold/stx-fault
```

Once finished, we will have it under:
https://software.opensuse.org/download.html?project=home:saulwold&package=stx-fault

### Important

#### Flags

- --keep-pkgs
- --prefer-pkgs

#### Commands

- linkpac: Link a package to another package

#### Things Changed

- We use Top Level Repository
  - Take stx-fault as an example
- Only change to CentOS Specfiles?
  - Build requirements
  - Runtime requirements
- Heavy clean up of Specfiles based in Spec Cleaner
  - See https://review.opendev.org/#/c/659157/
    - Check Zuul Check "flock-check-packaging": http://logs.openstack.org/57/659157/5/check/flock-check-packaging/1180044/
  - Do it locally
- Remove all the wheel stuff from SUSE
  - Suse build does not deal with wheels stuff
  - Let's not 
- SUSE wants ownership of directories to be declared, we need %dir

#### Build multiple spec files

- In top level directory
- Name? _multibuild

## openSUSE:Build Service Tutorial StarlingX marcelarosalesj stx-common

```sh
906ba71fcce3:~ # mkdir fm-common
906ba71fcce3:~ # osc co home:marcelarosalesj fm-common
A    home:marcelarosalesj
A    home:marcelarosalesj/fm-common
A    home:marcelarosalesj/fm-common/fm-common-0.0.tar.xz
A    home:marcelarosalesj/fm-common/fm-common.spec
A    home:marcelarosalesj/fm-common/fm-common_0.0-1.debian.tar.xz
A    home:marcelarosalesj/fm-common/fm-common_0.0-1.dsc
A    home:marcelarosalesj/fm-common/fm-common_0.0.orig.tar.gz
At revision 3.
```

```sh
906ba71fcce3:~ # ls home:marcelarosalesj/
fm-common
906ba71fcce3:~ # ls home:marcelarosalesj/fm-common/
fm-common-0.0.tar.xz  fm-common.spec  fm-common_0.0-1.debian.tar.xz  fm-common_0.0-1.dsc  fm-common_0.0.orig.tar.gz
```

```sh
906ba71fcce3:~ # cd home:marcelarosalesj/fm-common/
906ba71fcce3:~/home:marcelarosalesj/fm-common # osc build --no-verify fm-common.spec
```

```sh
906ba71fcce3:~/home:marcelarosalesj/fm-common # osc build --no-verify fm-common.spec
Building fm-common.spec for CentOS_7/x86_64
Getting buildinfo from server and store to /root/home:marcelarosalesj/fm-common/.osc/_buildinfo-CentOS_7-x86_64.xml
Getting buildconfig from server and store to /root/home:marcelarosalesj/fm-common/.osc/_buildconfig-CentOS_7-x86_64
Updating cache of required packages

The build root needs packages from project 'CentOS:CentOS-7'.
Note that malicious packages can compromise the build result or even your system.
Would you like to ...
0 - quit (default)
1 - always trust packages from 'CentOS:CentOS-7'
2 - trust packages just this time
? 
...
... # Taking around 10 minutes
...
[  347s] Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.yGJQVd
[  347s] + umask 022
[  347s] + cd /home/abuild/rpmbuild/BUILD
[  347s] + cd fm-common/sources
[  347s] + /usr/bin/rm -rf '/home/abuild/rpmbuild/BUILDROOT/fm-common-0.0-%{tis_patch_ver}.x86_64'
[  347s] + exit 0
[  347s] ... checking for files with abuild user/group
[  347s] 
[  347s] 906ba71fcce3 finished "build fm-common.spec" at Wed May 15 09:24:19 UTC 2019.
[  347s] 

/var/tmp/build-root/CentOS_7-x86_64/home/abuild/rpmbuild/SRPMS/fm-common-0.0-%{tis_patch_ver}.src.rpm

/var/tmp/build-root/CentOS_7-x86_64/home/abuild/rpmbuild/RPMS/x86_64/fm-common-0.0-%{tis_patch_ver}.x86_64.rpm
/var/tmp/build-root/CentOS_7-x86_64/home/abuild/rpmbuild/RPMS/x86_64/fm-common-doc-0.0-%{tis_patch_ver}.x86_64.rpm
/var/tmp/build-root/CentOS_7-x86_64/home/abuild/rpmbuild/RPMS/x86_64/fm-common-devel-0.0-%{tis_patch_ver}.x86_64.rpm
/var/tmp/build-root/CentOS_7-x86_64/home/abuild/rpmbuild/RPMS/x86_64/fm-common-debuginfo-0.0-%{tis_patch_ver}.x86_64.rpm
```

```sh
906ba71fcce3:~/home:marcelarosalesj/fm-common # ls
fm-common-0.0.tar.xz  fm-common.spec  fm-common_0.0-1.debian.tar.xz  fm-common_0.0-1.dsc  fm-common_0.0.orig.tar.gz
``` 

## openSUSE:Build Service Tutorial 

- https://en.opensuse.org/openSUSE:Build_Service_Tutorial

```sh
906ba71fcce3:~ # mkdir hello-world
906ba71fcce3:~ # cd hello-world/
906ba71fcce3:~/hello-world # osc checkout home:xe1gyq
A    home:xe1gyq
A    home:xe1gyq/hello-world
At revision None.
906ba71fcce3:~/hello-world # cd home:xe1gyq/
906ba71fcce3:~/hello-world/home:xe1gyq # 
```

```sh
906ba71fcce3:~/hello-world/home:xe1gyq # osc meta pkg -e home:xe1gyq hello-world
```

```xml
<package name="hello-world">

  <title>Hello World</title> <!-- Title of package -->

  <description>Package Hello World</description> <!-- for long description -->

<!-- following roles are inherited from the parent project
  <person role="maintainer" userid="xe1gyq"/>
  <person role="bugowner" userid="xe1gyq"/>
-->
<!--
  <url>PUT_UPSTREAM_URL_HERE</url>
-->

<!--
  use one of the examples below to disable building of this package
  on a certain architecture, in a certain repository,
  or a combination thereof:

  <disable arch="x86_64"/>
  <disable repository="SUSE_SLE-10"/>
  <disable repository="SUSE_SLE-10" arch="x86_64"/>

  Possible sections where you can use the tags above:
  <build>
  </build>
  <debuginfo>
  </debuginfo>
  <publish>
  </publish>
  <useforbuild>
  </useforbuild>

  Please have a look at:
  http://en.opensuse.org/Restricted_formats
  Packages containing formats listed there are NOT allowed to
  be packaged in the openSUSE Buildservice and will be deleted!

-->

</package>
```

```sh
906ba71fcce3:~/hello-world/home:xe1gyq # osc meta pkg -e home:xe1gyq hello-world
Sending meta data...
Done.
```

```sh
906ba71fcce3:~/hello-world/home:xe1gyq # osc co home:xe1gyq hello-world
A    home:xe1gyq
A    home:xe1gyq/hello-world
At revision None.
906ba71fcce3:~/hello-world/home:xe1gyq # ls
hello-world  home:xe1gyq
```

```sh
906ba71fcce3:~/hello-world/home:xe1gyq # cd hello-world/
906ba71fcce3:~/hello-world/home:xe1gyq/hello-world # ls
906ba71fcce3:~/hello-world/home:xe1gyq/hello-world # 
```

```
906ba71fcce3:~/hello-world/home:xe1gyq/hello-world # vim hello-world.spec
```

```sh
#
# spec file for package hello-world
#
# Copyright (c) 2019 SUSE LINUX Products GmbH, Nuernberg, Germany.
#
# All modifications and additions to the file contributed by third parties
# remain the property of their copyright owners, unless otherwise agreed
# upon. The license for this file, and modifications and additions to the
# file, is the same license as for the pristine package itself (unless the
# license for the pristine package is not an Open Source License, in which
# case the license is the MIT License). An "Open Source License" is a
# license that conforms to the Open Source Definition (Version 1.9)
# published by the Open Source Initiative.

# Please submit bugfixes or comments via http://bugs.opensuse.org/
#

Name:           hello-world
Version:        2.10
Release:        1
License:        GPLv3+
Summary:        The "Hello World" program from GNU
Url:            https://www.gnu.org/software/hello/
Group:
Source:         https://ftp.gnu.org/gnu/hello/hello-%{version}.tar.gz
Patch:
BuildRequires:
PreReq:
Provides:
BuildRoot:      %{_tmppath}/%{name}-%{version}-build

%description
The "Hello World" program, done with all bells and whistles of a proper FOSS
project, including configuration, build, internationalization, help files, etc.

%prep
%setup -q

%build
%configure
make %{?_smp_mflags}

%install
make install DESTDIR=%{buildroot} %{?_smp_mflags}

%post

%postun

%files
%defattr(-,root,root)
%doc ChangeLog README COPYING
```

```sh
906ba71fcce3:~/hello-world/home:xe1gyq/hello-world # osc add *
A    hello-world.spec
```

```sh
906ba71fcce3:~/hello-world/home:xe1gyq/hello-world # osc commit
```


```sh
Commit Package Hello World

--This line, and those below, will be ignored--

A    hello-world.spec

Diff for working copy: .
Index: hello-world.spec
===================================================================
--- hello-world.spec    (revision 0)
+++ hello-world.spec    (revision 0)
@@ -0,0 +1,52 @@
+#
+# spec file for package hello-world
```

```sh
Sending    hello-world.spec
Transmitting file data .
Committed revision 1.
```
