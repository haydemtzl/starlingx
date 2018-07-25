
# Development Environment Setup

## Install stx-tools project

```
ser@workstation:~$ git clone git://git.openstack.org/openstack/stx-tools
Cloning into 'stx-tools'...
remote: Counting objects: 440, done.
remote: Compressing objects: 100% (233/233), done.
remote: Total 440 (delta 274), reused 347 (delta 201)
Receiving objects: 100% (440/440), 138.17 KiB | 0 bytes/s, done.
Resolving deltas: 100% (274/274), done.
Checking connectivity... done.
user@workstation:~$ 
```

```sh
user@workstation:~$ cd $HOME/stx-tools/
```

```
user@workstation:~/stx-tools$ git log --pretty=oneline -5
f31f99ab91435c32c24a86efd2e697177074f4b7 Merge "policycoreutils-newrole was missing"
c3000c2936f56ad0257142d5c5bad3afc08c46cc Merge "upgrade linux-firmware RPM version"
bd293f4428019a99a15a42c0d6254b9d1e52542a Merge "Add release tools"
2d8ea40aa5b04dc166a53b21c0c62290b99a410c policycoreutils-newrole was missing
84843617a75150848a436efafc9054d308404c7a Policycoreutils needs a new version
user@workstation:~/stx-tools$ 
```

## Create a Workspace Directory

```
user@workstation:~$ mkdir -p $HOME/starlingx/
```

# Build the CentOS Mirror Repository

## Setup Repository Docker Container

```sh
user@workstation:~$ cd $HOME/stx-tools/centos-mirror-tools/
```

```
user@workstation:~/stx-tools/centos-mirror-tools$ docker build -t user:centos-mirror-repository -f Dockerfile .
Sending build context to Docker daemon  260.6kB
Step 1/8 : FROM centos:7.4.1708
7.4.1708: Pulling from library/centos
18b8eb7e7f01: Already exists 
Digest: sha256:2a61f8abd6250751c4b1dd3384a2bdd8f87e0e60d11c064b8a90e2e552fee2d7
Status: Downloaded newer image for centos:7.4.1708
 ---> 3afd47092a0e
Step 2/8 : WORKDIR /localdisk
 ---> Running in 9169f701a144
...
Step 7/8 : RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY*
 ---> Running in cb18d8b87c88
Removing intermediate container cb18d8b87c88
 ---> 3260766a8fb4
Step 8/8 : ENTRYPOINT ["/bin/bash"]
 ---> Running in 382ab5084676
Removing intermediate container 382ab5084676
 ---> 8e084ba7bb0e
Successfully built 8e084ba7bb0e
Successfully tagged user:centos-mirror-repository
user@workstation:~/stx-tools/centos-mirror-tools$ 
```

```
user@workstation:~/stx-tools/centos-mirror-tools$ docker run -it --name user-centos-mirror-repository -v $(pwd):/localdisk user:centos-mirror-repository
[root@ba54106f7bb4 localdisk]# 
```

## Download Packages

```
[root@ba54106f7bb4 localdisk]# bash download_mirror.sh 
--------------------------------------------------------------
WARNING: this script HAS TO access internet (http/https/ftp),
so please make sure your network working properly!!
step #1: start downloading RPMs/SRPMs from 3rd-party websites...
using ./rpms_from_3rd_parties.lst as the download name lists
2018-07-25_0914
Loaded plugins: fastestmirror, ovl
...
```

# Create StarlingX Packages

## Setup Building Docker Container

```
user@workstation:~/stx-tools$ mkdir -p $HOME/starlingx/workspace
user@workstation:~/stx-tools$ cd $HOME/stx-tools
user@workstation:~/stx-tools$ cp ~/.gitconfig toCOPY
user@workstation:~/stx-tools$ nano localrc
user@workstation:~/stx-tools$ 
```

```
# tbuilder localrc
MYUNAME=user
PROJECT=starlingx
HOST_PREFIX=$HOME/starlingx/workspace
HOST_MIRROR_DIR=$HOME/starlingx/mirror
```

```
user@workstation:~/stx-tools$ make base-build
docker build \
	--ulimit core=0 \
	--network host \
	-t local/dev-centos:7.3 \
	-f Dockerfile.centos73 \
	.
Sending build context to Docker daemon  2.085GB
Step 1/6 : FROM centos:7.3.1611
 ---> 66ee80d59a68
Step 2/6 : RUN yum install -y epel-release &&     yum install -y lighttpd lighttpd-fastcgi lighttpd-mod_geoip     sudo systemd     anaconda anaconda-help anaconda-runtime bc python-psutil createrepo /usr/bin/yumdownloader     /usr/bin/mkisofs git quilt pax perl-CPAN gcc expat-devel syslinux udisks2 rpm-build rpm-sign deltarpm     python-deltarpm rpm-python cpanminus wget     bind bind-utils squashfs-tools
 ---> Using cache
 ---> 4175c00ecde8
Step 3/6 : RUN cpanm --notest Fatal &&     cpanm --notest XML::SAX  &&     cpanm --notest XML::SAX::Expat &&     cpanm --notest XML::Parser &&     cpanm --notest XML::Simple
 ---> Using cache
 ---> 02c50642c96a
Step 4/6 : RUN yum install -y vim-enhanced openssl-devel gettext mongodb mongodb-server mariadb-devel python-testrepository     python-tox python-pep8 python-pip postgresql postgresql-devel python-devel libxml2 libxml2-devel libxslt-devel     libffi-devel sqlite-devel openldap-devel libvirt-devel python-subunit qemu-kvm
 ---> Using cache
 ---> fcd963800430
Step 5/6 : RUN pip install python-subunit junitxml --upgrade &&     pip install tox --upgrade
 ---> Using cache
 ---> e5d3c8a74d73
Step 6/6 : RUN curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/local/bin/repo &&     chmod a+x /usr/local/bin/repo
 ---> Using cache
 ---> 03797226f078
Successfully built 03797226f078
Successfully tagged local/dev-centos:7.3
user@workstation:~/stx-tools$ 
```

```
