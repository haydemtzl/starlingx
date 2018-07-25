
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
user@workstation:~/stx-tools$ make build
docker build \
	--build-arg MYUID=1000 \
	--build-arg MYUNAME=user \
	--ulimit core=0 \
	--network host \
	-t local/user-stx-builder:7.3 \
	-f Dockerfile.centos73.TC-builder \
	.
Sending build context to Docker daemon  2.085GB
Step 1/22 : FROM local/dev-centos:7.3
 ---> 03797226f078
Step 2/22 : MAINTAINER brian avery <brian.avery@intel.com>
 ---> Using cache
 ---> 03c7393a6d4d
Step 3/22 : ARG MYUID=1000
 ---> Using cache
 ---> b8b72333ab05
Step 4/22 : ARG MYUNAME=builder
 ---> Using cache
 ---> a0d0833ab89e
Step 5/22 : ENV container=docker
 ---> Using cache
 ---> 6cdce8d2a8e0
Step 6/22 : RUN groupadd -g 751 cgts &&     echo "mock:x:751:root" >> /etc/group &&     echo "mockbuild:x:9001:" >> /etc/group &&     yum install -y mock epel-release rpm-build &&     useradd -s /sbin/nologin -u 9001 -g 9001 mockbuild &&     rmdir /var/lib/mock &&     ln -s /localdisk/loadbuild/mock /var/lib/mock &&     rmdir /var/cache/mock &&     ln -s /localdisk/loadbuild/mock-cache /var/cache/mock &&     echo "config_opts['use_nspawn'] = False" >> /etc/mock/site-defaults.cfg &&     echo "config_opts['rpmbuild_networking'] = True" >> /etc/mock/site-defaults.cfg &&     echo  >> /etc/mock/site-defaults.cfg
 ---> Using cache
 ---> 9960c20674ea
Step 7/22 : COPY toCOPY/cgcs_overlay /opt/cgcs_overlay
 ---> Using cache
 ---> 70f836ab2322
Step 8/22 : RUN cd /opt/cgcs_overlay &&     make &&     make install
 ---> Using cache
 ---> bc7fab48d41f
Step 9/22 : RUN echo "# Load stx-builder configuration" >> /etc/profile.d/TC.sh &&     echo "if [[ -r \${HOME}/buildrc ]]; then" >> /etc/profile.d/TC.sh &&     echo "    source \${HOME}/buildrc" >> /etc/profile.d/TC.sh &&     echo "    export PROJECT SRC_BUILD_ENVIRONMENT MYPROJECTNAME MYUNAME" >> /etc/profile.d/TC.sh &&     echo "    export MY_BUILD_CFG MY_BUILD_CFG_RT MY_BUILD_CFG_STD MY_BUILD_DIR MY_BUILD_ENVIRONMENT MY_BUILD_ENVIRONMENT_FILE MY_BUILD_ENVIRONMENT_FILE_RT MY_BUILD_ENVIRONMENT_FILE_STD MY_DEBUG_BUILD_CFG_RT MY_DEBUG_BUILD_CFG_STD MY_LOCAL_DISK MY_MOCK_ROOT MY_REPO MY_REPO_ROOT_DIR MY_SRC_RPM_BUILD_DIR MY_TC_RELEASE MY_WORKSPACE" >> /etc/profile.d/TC.sh &&     echo "fi" >> /etc/profile.d/TC.sh &&     echo "export FORMAL_BUILD=0" >> /etc/profile.d/TC.sh &&     echo "export PATH=\$MY_REPO/build-tools:\$PATH" >> /etc/profile.d/TC.sh
 ---> Using cache
 ---> 8895ae8c6e6b
Step 10/22 : RUN localedef -i en_US -f UTF-8 en_US.UTF-8
 ---> Using cache
 ---> 8276e615b245
Step 11/22 : RUN mkdir -p /www/run &&     mkdir -p /www/logs &&     mkdir -p /www/home &&     mkdir -p /www/root/htdocs/localdisk &&     chown -R $MYUID:cgts /www &&     ln -s /localdisk/loadbuild /www/root/htdocs/localdisk/loadbuild &&     ln -s /import/mirrors/CentOS /www/root/htdocs/CentOS &&     ln -s /import/mirrors/fedora /www/root/htdocs/fedora &&     ln -s /localdisk/designer /www/root/htdocs/localdisk/designer
 ---> Using cache
 ---> e6ab2e5e118c
Step 12/22 : RUN echo "$MYUNAME ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers &&     mkdir -p  /var/log/lighttpd  &&     chmod a+rwx /var/log/lighttpd/ &&     sed -i 's%^var\.log_root.*$%var.log_root = "/www/logs"%g' /etc/lighttpd/lighttpd.conf  &&     sed -i 's%^var\.server_root.*$%var.server_root = "/www/root"%g' /etc/lighttpd/lighttpd.conf  &&     sed -i 's%^var\.home_dir.*$%var.home_dir = "/www/home"%g' /etc/lighttpd/lighttpd.conf  &&     sed -i 's%^var\.state_dir.*$%var.state_dir = "/www/run"%g' /etc/lighttpd/lighttpd.conf  &&     sed -i "s/server.port/#server.port/g" /etc/lighttpd/lighttpd.conf  &&     sed -i "s/server.use-ipv6/#server.use-ipv6/g" /etc/lighttpd/lighttpd.conf &&     sed -i "s/server.username/#server.username/g" /etc/lighttpd/lighttpd.conf &&     sed -i "s/server.groupname/#server.groupname/g" /etc/lighttpd/lighttpd.conf &&     sed -i "s/server.bind/#server.bind/g" /etc/lighttpd/lighttpd.conf &&     sed -i "s/server.document-root/#server.document-root/g" /etc/lighttpd/lighttpd.conf &&     sed -i "s/server.dirlisting/#server.dirlisting/g" /etc/lighttpd/lighttpd.conf &&     echo "server.port = 8088" >> /etc/lighttpd/lighttpd.conf &&     echo "server.use-ipv6 = \"disable\"" >> /etc/lighttpd/lighttpd.conf &&     echo "server.username = \"$MYUNAME\"" >> /etc/lighttpd/lighttpd.conf &&     echo "server.groupname = \"cgts\"" >> /etc/lighttpd/lighttpd.conf &&     echo "server.bind = \"localhost\"" >> /etc/lighttpd/lighttpd.conf &&     echo "server.document-root   = \"/www/root/htdocs\"" >> /etc/lighttpd/lighttpd.conf &&     sed -i "s/dir-listing.activate/#dir-listing.activate/g" /etc/lighttpd/conf.d/dirlisting.conf &&     echo "dir-listing.activate = \"enable\"" >> /etc/lighttpd/conf.d/dirlisting.conf
 ---> Using cache
 ---> aeaef99f78ec
Step 13/22 : RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done);     rm -f /lib/systemd/system/multi-user.target.wants/*;    rm -f /etc/systemd/system/*.wants/*;    rm -f /lib/systemd/system/local-fs.target.wants/*;     rm -f /lib/systemd/system/sockets.target.wants/*udev*;     rm -f /lib/systemd/system/sockets.target.wants/*initctl*;     rm -f /lib/systemd/system/basic.target.wants/*;    rm -f /lib/systemd/system/anaconda.target.wants/*
 ---> Using cache
 ---> 17590df78b09
Step 14/22 : VOLUME /run /tmp
 ---> Using cache
 ---> 0a4c19c3fc7f
Step 15/22 : RUN useradd -r -u $MYUID -g cgts -m $MYUNAME &&     ln -s /home/$MYUNAME/.ssh /mySSH
 ---> Using cache
 ---> af77ada98d09
Step 16/22 : COPY toCOPY/finishSetup.sh /usr/local/bin
 ---> Using cache
 ---> e09cd34cda10
Step 17/22 : COPY toCOPY/generate-cgcs-tis-repo /usr/local/bin
 ---> Using cache
 ---> 5842ae091aa9
Step 18/22 : COPY toCOPY/generate-cgcs-centos-repo.sh /usr/local/bin
 ---> Using cache
 ---> 501aa42eadc4
Step 19/22 : COPY toCOPY/.inputrc /home/$MYUNAME/
 ---> Using cache
 ---> 42ef1309c4a1
Step 20/22 : COPY toCOPY/.gitconfig /home/$MYUNAME/
 ---> Using cache
 ---> e6e22c1a1a77
Step 21/22 : RUN echo "bash -C /usr/local/bin/finishSetup.sh" >> /home/$MYUNAME/.bashrc &&     echo "export PATH=/usr/local/bin:/localdisk/designer/$MYUNAME/bin:\$PATH" >> /home/$MYUNAME/.bashrc &&     chmod a+x /usr/local/bin/*
 ---> Using cache
 ---> 94000ffd0e05
Step 22/22 : CMD /usr/sbin/init
 ---> Using cache
 ---> fcaf81e37805
Successfully built fcaf81e37805
Successfully tagged local/user-stx-builder:7.3
user@workstation:~/stx-tools$ 
```
