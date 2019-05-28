# Multi-OS

- mtce-storage
- platform-kickstarts
- sm-tools

## home:xe1gyq > mtce-storage

```sh
$ docker run -ti --privileged -v /proc:/proc -v /dev:/dev --name=osc-mtce-storage jaltek/docker-opensuse-osc-client /bin/bash
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
vim: No such file or directory
:/ # 
```

A second time

```sh
:/ # osc sr home:xe1gyq mtce-storage Cloud:StarlingX:2.0 mtce-storage
```

```sh
:/ # osc sr home:xe1gyq mtce-storage Cloud:StarlingX:2.0 mtce-storage
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
