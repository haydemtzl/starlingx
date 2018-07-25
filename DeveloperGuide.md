
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
