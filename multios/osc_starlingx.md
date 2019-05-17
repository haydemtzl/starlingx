# openSUSE:Open Build Service Command Line Tool StarlingX

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

## home:xe1gyq > platform-kickstarts

1. Under your workspace (e.g. https://build.opensuse.org/project/show/home:xe1gyq), select "Create Package"
2. Fill out and Accept:
   - Name: platform-kickstarts
   - Title: Platform kickstart files
   - Description: Platform kickstart files
3. 

```
user@workstation:~$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc-xe1gyq jaltek/docker-opensuse-osc-client /bin/bash
:/ # 
```

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

