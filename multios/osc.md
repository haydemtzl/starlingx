
# openSUSE:Open Build Service Command Line Tool

- https://build.opensuse.org/

## Setup

```sh
906ba71fcce3:/home:xe1gyq # zypper install vim
```

## openSUSE:Build Service Tutorial StarlingX

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

Now let's build

```sh
906ba71fcce3:~/stx-fault/home:saulwold/stx-fault # osc build --no-verify fm-common.spec
Building fm-common.spec for SLE_12_SP4/x86_64
Getting buildinfo from server and store to /root/stx-fault/home:saulwold/stx-fault/.osc/_buildinfo-SLE_12_SP4-x86_64.xml
...
...
```

Important Flags

- --keep-pkgs
- --prefer-pkgs

### Build multiple spec files

- In top level directory
- Name? _multibuild

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
