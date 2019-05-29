# Installing StarlingX with containers: Standard Storage configuration

## Setup Controller-0

### Install StarlingX

1. bootimage.iso
2. Use password _St4rlingX*_

### Bootstrap the controller

```sh
localhost:~$ ip a
```

```sh
localhost:~$ sudo ip address add 10.10.10.10/24 dev enp2s1
localhost:~$ sudo ip link set up dev enp2s1
localhost:~$ sudo ip route add default via 10.10.10.1 dev enp2s1
localhost:~$ ping 8.8.8.8
localhost:~$ ping 10.248.2.1
localhost:~$ ping 10.22.224.196
```

```sh
user@workstation:~$ ssh wrsroot@10.10.10.10

WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

localhost:~$ 
````

```sh
localhost:~$ vi /home/wrsroot/localhost.yml
```

```yaml
system_mode: duplex

dns_servers:
  - 10.248.2.1
  - 10.22.224.196

docker_http_proxy: http://proxy-chain.intel.com:911
docker_https_proxy: http://proxy-chain.intel.com:912
docker_no_proxy:
  - localhost
  - 127.0.0.1
  - 192.168.204.2
  - 192.168.204.3
  - 192.168.204.4
  - 10.10.10.3
  - 10.10.10.4
  - 10.10.10.5

external_oam_subnet: 10.10.10.0/24
external_oam_gateway_address: 10.10.10.1
external_oam_floating_address: 10.10.10.3

ansible_become_pass: St4rlingX*
admin_password: St4rlingX*
```

```sh
localhost:~$ ansible-playbook /usr/share/ansible/stx-ansible/playbooks/bootstrap/bootstrap.yml -e "ansible_become_pass=St4rlingX*"
```

### Provisioning Controller-0

#### Configure OAM, Management and Cluster interfaces

```sh
user@workstation:~/stx-tools/deployment/libvirt$ ssh wrsroot@10.10.10.10
The authenticity of host '10.10.10.10 (10.10.10.10)' can't be established.
ECDSA key fingerprint is SHA256:zmpmzT51KHw5VgFnH4J5fwAi5Q1hsYv2QSu03pKudyY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.10.10' (ECDSA) to the list of known hosts.
Release 19.01
------------------------------------------------------------------------
W A R N I N G *** W A R N I N G *** W A R N I N G *** W A R N I N G *** 
------------------------------------------------------------------------
THIS IS A PRIVATE COMPUTER SYSTEM.
This computer system including all related equipment, network devices
(specifically including Internet access), are provided only for authorized use.
All computer systems may be monitored for all lawful purposes, including to
ensure that their use is authorized, for management of the system, to
facilitate protection against unauthorized access, and to verify security
procedures, survivability and operational security. Monitoring includes active
attacks by authorized personnel and their entities to test or verify the
security of the system. During monitoring, information may be examined,
recorded, copied and used for authorized purposes. All information including
personal information, placed on or sent over this system may be monitored. Uses
of this system, authorized or unauthorized, constitutes consent to monitoring
of this system. Unauthorized use may subject you to criminal prosecution.
Evidence of any such unauthorized use collected during monitoring may be used
for administrative, criminal or other adverse action. Use of this system
constitutes consent to monitoring for these purposes.

wrsroot@10.10.10.10's password: 
Last login: Wed May 29 08:27:52 2019

WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

controller-0:~$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ OAM_IF=enp2s1
[wrsroot@controller-0 ~(keystone_admin)]$ MGMT_IF=enp2s2
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 lo -c none
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | lo                                   |
| iftype       | virtual                              |
| ports        | []                                   |
| datanetworks | []                                   |
| imac         | 00:00:00:00:00:00                    |
| imtu         | 1500                                 |
| ifclass      | None                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 5b06a590-f731-4307-8cc1-1ce20084763f |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:37:15.412532+00:00     |
| updated_at   | 2019-05-29T08:47:33.759353+00:00     |
| sriov_numvfs | 0                                    |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 $OAM_IF --networks oam -c platform

+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | enp2s1                               |
| iftype       | ethernet                             |
| ports        | [u'enp2s1']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:f6:f5:3a                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | oam                                  |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 231c2c43-833f-4125-bc11-1e18a89d1ea1 |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:35:21.591815+00:00     |
| updated_at   | 2019-05-29T08:48:13.362198+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 $MGMT_IF -c platform --networks mgmt
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | enp2s2                               |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:36:bc:90                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | mgmt                                 |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | cef6994a-5033-42ad-a4f4-09645347c73e |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:35:21.714315+00:00     |
| updated_at   | 2019-05-29T08:48:15.564040+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-0 $MGMT_IF -c platform --networks cluster-host
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | enp2s2                               |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:36:bc:90                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | cef6994a-5033-42ad-a4f4-09645347c73e |
| ihost_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T08:35:21.714315+00:00     |
| updated_at   | 2019-05-29T08:48:18.168298+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ system ntp-modify ntpservers=0.pool.ntp.org,1.pool.ntp.org
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| uuid         | 257f46a1-c315-43de-8c09-b08155dcb69b |
| enabled      | True                                 |
| ntpservers   | 0.pool.ntp.org,1.pool.ntp.org        |
| isystem_uuid | d046949c-e16e-4475-bdee-f420a430fb00 |
| created_at   | 2019-05-29T08:34:05.340438+00:00     |
| updated_at   | None                                 |
+--------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system modify --vswitch_type ovs-dpdk
+----------------------+--------------------------------------+
| Property             | Value                                |
+----------------------+--------------------------------------+
| contact              | None                                 |
| created_at           | 2019-05-29T08:34:05.314720+00:00     |
| description          | None                                 |
| https_enabled        | False                                |
| location             | None                                 |
| name                 | a6b7c820-ef13-4260-b864-17b72c593898 |
| region_name          | RegionOne                            |
| sdn_enabled          | False                                |
| security_feature     | spectre_meltdown_v1                  |
| service_project_name | services                             |
| software_version     | 19.01                                |
| system_mode          | duplex                               |
| system_type          | Standard                             |
| timezone             | UTC                                  |
| updated_at           | 2019-05-29T08:35:15.459547+00:00     |
| uuid                 | d046949c-e16e-4475-bdee-f420a430fb00 |
| vswitch_type         | ovs-dpdk                             |
+----------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system modify --vswitch_type ovs
+----------------------+--------------------------------------+
| Property             | Value                                |
+----------------------+--------------------------------------+
| contact              | None                                 |
| created_at           | 2019-05-29T08:34:05.314720+00:00     |
| description          | None                                 |
| https_enabled        | False                                |
| location             | None                                 |
| name                 | a6b7c820-ef13-4260-b864-17b72c593898 |
| region_name          | RegionOne                            |
| sdn_enabled          | False                                |
| security_feature     | spectre_meltdown_v1                  |
| service_project_name | services                             |
| software_version     | 19.01                                |
| system_mode          | duplex                               |
| system_type          | Standard                             |
| timezone             | UTC                                  |
| updated_at           | 2019-05-29T08:48:56.934789+00:00     |
| uuid                 | d046949c-e16e-4475-bdee-f420a430fb00 |
| vswitch_type         | ovs                                  |
+----------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-0 openstack-control-plane=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | d43c36fa-8f5a-4e50-ba8c-d54b894d8c01 |
| host_uuid   | b0f5e498-3e83-4c3e-9404-0dac97913b8a |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-0
+---------------------+--------------------------------------------+
| Property            | Value                                      |
+---------------------+--------------------------------------------+
| action              | none                                       |
| administrative      | locked                                     |
| availability        | online                                     |
| bm_ip               | None                                       |
| bm_type             | None                                       |
| bm_username         | None                                       |
| boot_device         | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| capabilities        | {u'stor_function': u'monitor'}             |
| config_applied      | 41d00c71-4a6d-4ae3-8db8-0f153060251d       |
| config_status       | None                                       |
| config_target       | 8ababfce-3309-4745-96ad-feb7167c99f8       |
| console             | tty0                                       |
| created_at          | 2019-05-29T08:35:18.384310+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 52:54:00:36:bc:90                          |
| operational         | disabled                                   |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 19.01                                      |
| task                | Unlocking                                  |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2019-05-29T08:50:08.683148+00:00           |
| uptime              | 1966                                       |
| uuid                | b0f5e498-3e83-4c3e-9404-0dac97913b8a       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

```sh
user@workstation:~/stx-tools/deployment/libvirt$ ssh wrsroot@10.10.10.3                                                                                                         
The authenticity of host '10.10.10.3 (10.10.10.3)' can't be established.
ECDSA key fingerprint is SHA256:zmpmzT51KHw5VgFnH4J5fwAi5Q1hsYv2QSu03pKudyY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '10.10.10.3' (ECDSA) to the list of known hosts.
Release 19.01
------------------------------------------------------------------------
W A R N I N G *** W A R N I N G *** W A R N I N G *** W A R N I N G *** 
------------------------------------------------------------------------
THIS IS A PRIVATE COMPUTER SYSTEM.
This computer system including all related equipment, network devices
(specifically including Internet access), are provided only for authorized use.
All computer systems may be monitored for all lawful purposes, including to
ensure that their use is authorized, for management of the system, to
facilitate protection against unauthorized access, and to verify security
procedures, survivability and operational security. Monitoring includes active
attacks by authorized personnel and their entities to test or verify the
security of the system. During monitoring, information may be examined,
recorded, copied and used for authorized purposes. All information including
personal information, placed on or sent over this system may be monitored. Uses
of this system, authorized or unauthorized, constitutes consent to monitoring
of this system. Unauthorized use may subject you to criminal prosecution.
Evidence of any such unauthorized use collected during monitoring may be used
for administrative, criminal or other adverse action. Use of this system
constitutes consent to monitoring for these purposes.

wrsroot@10.10.10.3's password: 
Last login: Wed May 29 08:47:20 2019 from 10.10.10.1
/etc/motd.d/00-header:

WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

(reverse-i-search)`': ^C                                                                                                                                                        
controller-0:~$ system host-list
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ sudo ufw disable^C
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 2 personality=controller
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:10:07.675773+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:2f:73:a8                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 52fbdff9-f575-42ea-b882-20fecebc3a7c |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 3 personality=worker hostname=compute-0                                                                            
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:12:09.750282+00:00     |
| hostname            | compute-0                            |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.8                        |
| mgmt_mac            | 52:54:00:77:70:e6                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 4 personality=worker hostname=compute-1
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+                                                                                                       [1174/1930]
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:13:01.680278+00:00     |
| hostname            | compute-1                            |
| id                  | 4                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.14                       |
| mgmt_mac            | 52:54:00:69:a0:5c                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | compute-1    | worker      | locked         | disabled    | offline      |
| 5  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 5 personality=storage                                                                                              
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:13:53.664236+00:00     |
| hostname            | storage-0                            |
| id                  | 5                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.7                        |
| mgmt_mac            | 52:54:00:df:ef:b5                    |
| operational         | disabled                             |
| personality         | storage                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 1190f0c2-1c5c-4017-95a0-f0c46c2ac2df |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | compute-1    | worker      | locked         | disabled    | offline      |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | compute-1    | worker      | locked         | disabled    | offline      |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | compute-1    | worker      | locked         | disabled    | offline      |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
| 6  | None         | None        | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-update 6 personality=storage                                                                                              
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | offline                              |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:14:28.888055+00:00     |
| hostname            | storage-1                            |
| id                  | 6                                    |
| install_output      | text                                 |
| install_state       | None                                 |
| install_state_info  | None                                 |
| invprovision        | None                                 |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.12                       |
| mgmt_mac            | 52:54:00:31:f2:21                    |
| operational         | disabled                             |
| personality         | storage                              |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | None                                 |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | None                                 |
| uptime              | 0                                    |
| uuid                | 5353214c-686e-47ef-847c-5cba7273290d |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | offline      |
| 4  | compute-1    | worker      | locked         | disabled    | offline      |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
| 6  | storage-1    | storage     | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | locked         | disabled    | online       |
| 3  | compute-0    | worker      | locked         | disabled    | online       |
| 4  | compute-1    | worker      | locked         | disabled    | online       |
| 5  | storage-0    | storage     | locked         | disabled    | offline      |
| 6  | storage-1    | storage     | locked         | disabled    | offline      |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-1 openstack-control-plane=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 32f27647-18a6-41be-97f6-94666db12acc |
| host_uuid   | 52fbdff9-f575-42ea-b882-20fecebc3a7c |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for NODE in compute-0 compute-1; do
>   system host-label-assign $NODE  openstack-compute-node=enabled
>   system host-label-assign $NODE  openvswitch=enabled
>   system host-label-assign $NODE  sriov=enabled
> done

+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 4f4b6ae8-2a94-491e-80f6-880bb8245b5d |
| host_uuid   | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| label_key   | openstack-compute-node               |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 97c38ef8-9a40-4e78-a13f-4a927718675c |
| host_uuid   | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| label_key   | openvswitch                          |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 2589d9ac-0a8e-4efa-b7c3-a5aa475ed601 |
| host_uuid   | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| label_key   | sriov                                |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 740a8980-b680-4565-81e3-188d2b50c4f0 |
| host_uuid   | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| label_key   | openstack-compute-node               |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 8d131803-c682-4dcf-9bf2-261573b5f6db |
| host_uuid   | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| label_key   | openvswitch                          |
| label_value | enabled                              |
+-------------+--------------------------------------+
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | a1f51dc6-1d73-4693-8e31-d018cee1f549 |
| host_uuid   | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| label_key   | sriov                                |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -n oam0 -c platform --networks oam controller-1 $(system host-if-list -a controller-1 | awk '/enp2s1/{print $2}'
)
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | oam0                                 |
| iftype       | ethernet                             |
| ports        | [u'enp2s1']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:44:7e:63                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | oam                                  |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | a2c8cf1f-d495-40b5-9c24-811d42579314 |
| ihost_uuid   | 52fbdff9-f575-42ea-b882-20fecebc3a7c |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:21:20.955989+00:00     |
| updated_at   | 2019-05-29T09:25:51.761220+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify controller-1 mgmt0 --networks cluster-host
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:2f:73:a8                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 4855f7aa-9b3c-4813-aa5f-6a36c8de591d |
| ihost_uuid   | 52fbdff9-f575-42ea-b882-20fecebc3a7c |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:21:21.873086+00:00     |
| updated_at   | 2019-05-29T09:25:58.202780+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock controller-1
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {u'stor_function': u'monitor'}       |
| config_applied      | None                                 |
| config_status       | None                                 |
| config_target       | None                                 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:10:07.675773+00:00     |
| hostname            | controller-1                         |
| id                  | 2                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.4                        |
| mgmt_mac            | 52:54:00:2f:73:a8                    |
| operational         | disabled                             |
| personality         | controller                           |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-29T09:25:45.387277+00:00     |
| uptime              | 268                                  |
| uuid                | 52fbdff9-f575-42ea-b882-20fecebc3a7c |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list                                                                                                                      
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | disabled    | offline      |
| 3  | compute-0    | worker      | locked         | disabled    | online       |
| 4  | compute-1    | worker      | locked         | disabled    | online       |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | degraded     |
| 3  | compute-0    | worker      | locked         | disabled    | online       |
| 4  | compute-1    | worker      | locked         | disabled    | online       |
| 5  | storage-0    | storage     | locked         | disabled    | online       |
| 6  | storage-1    | storage     | locked         | disabled    | online       |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     88457a91-6b7f-4069-917d-03f7adc9366a
    health: HEALTH_WARN
            Reduced data availability: 64 pgs inactive
 
  services:
    mon: 2 daemons, quorum controller-0,controller-1
    mgr: controller-0(active), standbys: controller-1
    osd: 0 osds: 0 up, 0 in
 
  data:
    pools:   1 pools, 64 pgs
    objects: 0  objects, 0 B
    usage:   0 B used, 0 B / 0 B avail
    pgs:     100.000% pgs unknown
             64 unknown

```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host storage-0 $(system host-if-list -a storage-0 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:df:ef:b5                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | fc42680f-da82-4711-bd3d-d2779d0c74a4 |
| ihost_uuid   | 1190f0c2-1c5c-4017-95a0-f0c46c2ac2df |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:25:15.939175+00:00     |
| updated_at   | 2019-05-29T09:36:35.224269+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host storage-1 $(system host-if-list -a storage-1 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:31:f2:21                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | daaa7a48-9880-4a8b-b4d6-e3e11cc37422 |
| ihost_uuid   | 5353214c-686e-47ef-847c-5cba7273290d |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:25:40.480775+00:00     |
| updated_at   | 2019-05-29T09:36:44.976431+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-0 $(system host-disk-list storage-0 | awk '/sdb/{print $2}')
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 0                                                |
| function         | osd                                              |
| state            | configuring-on-unlock                            |
| journal_location | f5215a6f-1807-4293-b935-1bf3300d1722             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | f5215a6f-1807-4293-b935-1bf3300d1722             |
| ihost_uuid       | 1190f0c2-1c5c-4017-95a0-f0c46c2ac2df             |
| idisk_uuid       | b2c41d26-880b-4850-b841-6be8ed355201             |
| tier_uuid        | c478adaf-97b1-4faa-8054-435f47c5022f             |
| tier_name        | storage                                          |
| created_at       | 2019-05-29T09:36:56.225007+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-stor-add storage-1 $(system host-disk-list storage-1 | awk '/sdb/{print $2}')
+------------------+--------------------------------------------------+
| Property         | Value                                            |
+------------------+--------------------------------------------------+
| osdid            | 1                                                |
| function         | osd                                              |
| state            | configuring-on-unlock                            |
| journal_location | d75a341a-1a4a-42a2-a543-0fa5c54c801c             |
| journal_size_gib | 1024                                             |
| journal_path     | /dev/disk/by-path/pci-0000:00:1f.2-ata-2.0-part2 |
| journal_node     | /dev/sdb2                                        |
| uuid             | d75a341a-1a4a-42a2-a543-0fa5c54c801c             |
| ihost_uuid       | 5353214c-686e-47ef-847c-5cba7273290d             |
| idisk_uuid       | 40b1bc3d-70f1-41f3-b742-89e08231aaca             |
| tier_uuid        | c478adaf-97b1-4faa-8054-435f47c5022f             |
| tier_name        | storage                                          |
| created_at       | 2019-05-29T09:37:02.283811+00:00                 |
| updated_at       | None                                             |
+------------------+--------------------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-0
+---------------------+---------------------------------------------------------------+
| Property            | Value                                                         |
+---------------------+---------------------------------------------------------------+
| action              | none                                                          |
| administrative      | locked                                                        |
| availability        | online                                                        |
| bm_ip               | None                                                          |
| bm_type             | None                                                          |
| bm_username         | None                                                          |
| boot_device         | sda                                                           |
| capabilities        | {u'stor_function': u'monitor'}                                |
| config_applied      | None                                                          |
| config_status       | Config out-of-date                                            |
| config_target       | a18000ba-0696-46cd-a44e-da4c3203bd44                          |
| console             | ttyS0,115200                                                  |
| created_at          | 2019-05-29T09:13:53.664236+00:00                              |
| hostname            | storage-0                                                     |
| id                  | 5                                                             |
| install_output      | text                                                          |
| install_state       | completed                                                     |
| install_state_info  | None                                                          |
| invprovision        | unprovisioned                                                 |
| location            | {}                                                            |
| mgmt_ip             | 192.168.204.7                                                 |
| mgmt_mac            | 52:54:00:df:ef:b5                                             |
| operational         | disabled                                                      |
| peers               | {u'hosts': [u'storage-1', u'storage-0'], u'name': u'group-0'} |
| personality         | storage                                                       |
| reserved            | False                                                         |
| rootfs_device       | sda                                                           |
| serialid            | None                                                          |
| software_load       | 19.01                                                         |
| task                | Unlocking                                                     |
| tboot               | false                                                         |
| ttys_dcd            | None                                                          |
| updated_at          | 2019-05-29T09:37:10.589276+00:00                              |
| uptime              | 753                                                           |
| uuid                | 1190f0c2-1c5c-4017-95a0-f0c46c2ac2df                          |
| vim_progress_status | None                                                          |
+---------------------+---------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-unlock storage-1
+---------------------+---------------------------------------------------------------+
| Property            | Value                                                         |
+---------------------+---------------------------------------------------------------+
| action              | none                                                          |
| administrative      | locked                                                        |
| availability        | online                                                        |
| bm_ip               | None                                                          |
| bm_type             | None                                                          |
| bm_username         | None                                                          |
| boot_device         | sda                                                           |
| capabilities        | {}                                                            |
| config_applied      | None                                                          |
| config_status       | Config out-of-date                                            |
| config_target       | e81ff4ce-25e3-4af1-a96c-0fba13a29dd8                          |
| console             | ttyS0,115200                                                  |
| created_at          | 2019-05-29T09:14:28.888055+00:00                              |
| hostname            | storage-1                                                     |
| id                  | 6                                                             |
| install_output      | text                                                          |
| install_state       | completed                                                     |
| install_state_info  | None                                                          |
| invprovision        | unprovisioned                                                 |
| location            | {}                                                            |
| mgmt_ip             | 192.168.204.12                                                |
| mgmt_mac            | 52:54:00:31:f2:21                                             |
| operational         | disabled                                                      |
| peers               | {u'hosts': [u'storage-0', u'storage-1'], u'name': u'group-0'} |
| personality         | storage                                                       |
| reserved            | False                                                         |
| rootfs_device       | sda                                                           |
| serialid            | None                                                          |
| software_load       | 19.01                                                         |
| task                | Unlocking                                                     |
| tboot               | false                                                         |
| ttys_dcd            | None                                                          |
| updated_at          | 2019-05-29T09:37:10.588845+00:00                              |
| uptime              | 720                                                           |
| uuid                | 5353214c-686e-47ef-847c-5cba7273290d                          |
| vim_progress_status | None                                                          |
+---------------------+---------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | degraded     |
| 3  | compute-0    | worker      | locked         | disabled    | online       |
| 4  | compute-1    | worker      | locked         | disabled    | online       |
| 5  | storage-0    | storage     | unlocked       | disabled    | intest       |
| 6  | storage-1    | storage     | unlocked       | disabled    | intest       |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ ceph -s
  cluster:
    id:     88457a91-6b7f-4069-917d-03f7adc9366a
    health: HEALTH_OK
 
  services:
    mon: 3 daemons, quorum controller-0,controller-1,storage-0
    mgr: controller-0(active), standbys: controller-1
    osd: 2 osds: 2 up, 2 in
    rgw: 1 daemon active
 
  data:
    pools:   4 pools, 256 pgs
    objects: 1.13 k objects, 1.1 KiB
    usage:   225 MiB used, 398 GiB / 398 GiB avail
    pgs:     256 active+clean
 
  io:
    client:   9.3 KiB/s rd, 0 B/s wr, 13 op/s rd, 9 op/s wr 

```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host compute-0 $(system host-if-list -a compute-0 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:77:70:e6                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 196ce0c6-b8a5-40ce-92e7-55bc46d0cb2f |
| ihost_uuid   | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:23:43.241306+00:00     |
| updated_at   | 2019-05-29T09:41:32.791324+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-if-modify -c platform --networks cluster-host compute-1 $(system host-if-list -a compute-1 | awk '/mgmt0/{print $2}')
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:69:a0:5c                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | fb2a30f1-cb75-48fc-b513-74c16181d7e1 |
| ihost_uuid   | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:24:49.252601+00:00     |
| updated_at   | 2019-05-29T09:41:42.254207+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ DATA0IF=eth1000
[wrsroot@controller-0 ~(keystone_admin)]$ DATA1IF=eth1001
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET0='physnet0'
[wrsroot@controller-0 ~(keystone_admin)]$ PHYSNET1='physnet1'
[wrsroot@controller-0 ~(keystone_admin)]$ SPL=/tmp/tmp-system-port-list
[wrsroot@controller-0 ~(keystone_admin)]$ SPIL=/tmp/tmp-system-host-if-list
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET0} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 1                                    |
| uuid         | 3068d692-8745-4d8e-9a4b-fd5efadb8ebe |
| name         | physnet0                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system datanetwork-add ${PHYSNET1} vlan
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| id           | 2                                    |
| uuid         | e9cfbd7e-53fd-45af-9605-846f5fc32b75 |
| name         | physnet1                             |
| network_type | vlan                                 |
| mtu          | 1500                                 |
| description  | None                                 |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>   echo "Configuring interface for: $COMPUTE"
>   set -ex
>   system host-port-list ${COMPUTE} --nowrap > ${SPL}
>   system host-if-list -a ${COMPUTE} --nowrap > ${SPIL}
>   DATA0PCIADDR=$(cat $SPL | grep $DATA0IF |awk '{print $8}')
>   DATA1PCIADDR=$(cat $SPL | grep $DATA1IF |awk '{print $8}')
>   DATA0PORTUUID=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $2}')
>   DATA1PORTUUID=$(cat $SPL | grep ${DATA1PCIADDR} | awk '{print $2}')
>   DATA0PORTNAME=$(cat $SPL | grep ${DATA0PCIADDR} | awk '{print $4}')
>   DATA1PORTNAME=$(cat  $SPL | grep ${DATA1PCIADDR} | awk '{print $4}')
>   DATA0IFUUID=$(cat $SPIL | awk -v DATA0PORTNAME=$DATA0PORTNAME '($12 ~ DATA0PORTNAME) {print $2}')
>   DATA1IFUUID=$(cat $SPIL | awk -v DATA1PORTNAME=$DATA1PORTNAME '($12 ~ DATA1PORTNAME) {print $2}')
>   system host-if-modify -m 1500 -n data0 -d ${PHYSNET0} -c data ${COMPUTE} ${DATA0IFUUID}
>   system host-if-modify -m 1500 -n data1 -d ${PHYSNET1} -c data ${COMPUTE} ${DATA1IFUUID}
>   set +ex
> done
Configuring interface for: compute-0
+ system host-port-list compute-0 --nowrap
+ system host-if-list -a compute-0 --nowrap
++ cat /tmp/tmp-system-port-list
++ awk '{print $8}'
++ grep --color=auto eth1000
+ DATA0PCIADDR=0000:02:03.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1001
++ awk '{print $8}'
+ DATA1PCIADDR=0000:02:04.0
++ grep --color=auto 0000:02:03.0
++ awk '{print $2}'
++ cat /tmp/tmp-system-port-list
+ DATA0PORTUUID=559c0d10-24f1-419e-98a2-96664ebac145
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $2}'                                                                                                                                                   [403/1930]
+ DATA1PORTUUID=0fe052f3-dc1c-4d51-a2c5-7a3728e450b5
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $4}'
+ DATA0PORTNAME=eth1000
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $4}'
+ DATA1PORTNAME=eth1001
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA0PORTNAME=eth1000 '($12 ~ DATA0PORTNAME) {print $2}'
+ DATA0IFUUID=247605bd-0e62-4628-bee5-3ae57ca5fb87
++ awk -v DATA1PORTNAME=eth1001 '($12 ~ DATA1PORTNAME) {print $2}'
++ cat /tmp/tmp-system-host-if-list
+ DATA1IFUUID=43bd3a7c-4181-49d2-a8f7-e15db0902d82
+ system host-if-modify -m 1500 -n data0 -d physnet0 -c data compute-0 247605bd-0e62-4628-bee5-3ae57ca5fb87
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data0                                |
| iftype       | ethernet                             |
| ports        | [u'eth1000']                         |
| datanetworks | [u'physnet0']                        |
| imac         | 52:54:00:36:7f:de                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 247605bd-0e62-4628-bee5-3ae57ca5fb87 |
| ihost_uuid   | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:23:43.470520+00:00     |
| updated_at   | 2019-05-29T09:42:31.058061+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ system host-if-modify -m 1500 -n data1 -d physnet1 -c data compute-0 43bd3a7c-4181-49d2-a8f7-e15db0902d82
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data1                                |
| iftype       | ethernet                             |
| ports        | [u'eth1001']                         |
| datanetworks | [u'physnet1']                        |
| imac         | 52:54:00:05:d6:54                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 43bd3a7c-4181-49d2-a8f7-e15db0902d82 |
| ihost_uuid   | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:23:43.784523+00:00     |
| updated_at   | 2019-05-29T09:42:34.259687+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ set +ex
Configuring interface for: compute-1
+ system host-port-list compute-1 --nowrap
+ system host-if-list -a compute-1 --nowrap
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1000
++ awk '{print $8}'
+ DATA0PCIADDR=0000:02:03.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto eth1001
++ awk '{print $8}'
+ DATA1PCIADDR=0000:02:04.0
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:03.0
++ awk '{print $2}'
+ DATA0PORTUUID=91026ab6-8399-4c39-85f3-9c249b76106f                                                                                                                  [318/1930]
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $2}'
+ DATA1PORTUUID=918b89bd-9cb4-49a5-801f-435f969c87d4
++ grep --color=auto 0000:02:03.0
++ cat /tmp/tmp-system-port-list
++ awk '{print $4}'
+ DATA0PORTNAME=eth1000
++ cat /tmp/tmp-system-port-list
++ grep --color=auto 0000:02:04.0
++ awk '{print $4}'
+ DATA1PORTNAME=eth1001
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA0PORTNAME=eth1000 '($12 ~ DATA0PORTNAME) {print $2}'
+ DATA0IFUUID=ad62b8ab-a6ae-42d6-84aa-e2f9fd8dcf60
++ cat /tmp/tmp-system-host-if-list
++ awk -v DATA1PORTNAME=eth1001 '($12 ~ DATA1PORTNAME) {print $2}'
+ DATA1IFUUID=d61d1b70-c1df-4f92-a389-eacc15159706
+ system host-if-modify -m 1500 -n data0 -d physnet0 -c data compute-1 ad62b8ab-a6ae-42d6-84aa-e2f9fd8dcf60
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data0                                |
| iftype       | ethernet                             |
| ports        | [u'eth1000']                         |
| datanetworks | [u'physnet0']                        |
| imac         | 52:54:00:ec:d9:79                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | ad62b8ab-a6ae-42d6-84aa-e2f9fd8dcf60 |
| ihost_uuid   | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:24:49.572714+00:00     |
| updated_at   | 2019-05-29T09:42:39.940572+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ system host-if-modify -m 1500 -n data1 -d physnet1 -c data compute-1 d61d1b70-c1df-4f92-a389-eacc15159706
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | data1                                |
| iftype       | ethernet                             |
| ports        | [u'eth1001']                         |
| datanetworks | [u'physnet1']                        |
| imac         | 52:54:00:8a:42:a8                    |
| imtu         | 1500                                 |
| ifclass      | data                                 |
| networks     |                                      |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | d61d1b70-c1df-4f92-a389-eacc15159706 |
| ihost_uuid   | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:24:49.895596+00:00     |
| updated_at   | 2019-05-29T09:42:42.259706+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | disabled                             |
| ipv6_mode    | disabled                             |
| accelerated  | [True]                               |
+--------------+--------------------------------------+
+ set +ex
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>   ROOT_DISK=$(system host-show ${COMPUTE} | grep rootfs | awk '{print $4}')
>   ROOT_DISK_UUID=$(system host-disk-list ${COMPUTE} --nowrap | awk /${ROOT_DISK}/'{print $2}')
>   AVAIL_SIZE=$(system host-disk-list ${COMPUTE} | awk /${ROOT_DISK}/'{printf("%d",$12)}')
>   CGTS_PARTITION_SIZE=4
>   NOVA_PARTITION_SIZE=$(($AVAIL_SIZE - $CGTS_PARTITION_SIZE))
>   CGTS_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${CGTS_PARTITION_SIZE})
>   CGTS_PARTITION_UUID=$(echo ${CGTS_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}')
>   NOVA_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${NOVA_PARTITION_SIZE})
>   NOVA_PARTITION_UUID=$(echo ${NOVA_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}')
>   system host-lvg-add ${COMPUTE} cgts-vg
>   system host-pv-add ${COMPUTE} cgts-vg  ${CGTS_PARTITION_UUID}
>   system host-lvg-add ${COMPUTE} nova-local
>   system host-pv-add ${COMPUTE} nova-local ${NOVA_PARTITION_UUID}
>   system host-lvg-modify -b image ${COMPUTE} nova-local
> done
cgts-vg volume group already exists
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 8f9ed060-19cb-4173-9707-142fe8793dbb             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 3f02208d-f205-4857-b9bf-681b7806c379             |
| disk_or_part_device_node | /dev/sda5                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| lvm_pv_name              | /dev/sda5                                        |
| lvm_vg_name              | cgts-vg                                          |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | b27ddfa9-9d87-4580-ad09-dcc83056195d             |
| created_at               | 2019-05-29T09:43:15.123856+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 5a8feae2-3caa-4067-ab97-f26b5d05fd41                              |
| ihost_uuid            | b27ddfa9-9d87-4580-ad09-dcc83056195d                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T09:43:20.054730+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 2c6e4fda-2ac0-4bc0-8d48-23b23009d6d1             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 4b73f567-8666-4d76-8b74-75c18dec1487             |
| disk_or_part_device_node | /dev/sda6                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 |
| lvm_pv_name              | /dev/sda6                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | b27ddfa9-9d87-4580-ad09-dcc83056195d             |
| created_at               | 2019-05-29T09:43:22.068603+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 5a8feae2-3caa-4067-ab97-f26b5d05fd41                              |
| ihost_uuid            | b27ddfa9-9d87-4580-ad09-dcc83056195d                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T09:43:20.054730+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
cgts-vg volume group already exists
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | c89c1e41-fbe2-46d9-98b9-9d5c2ff04788             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | 1c4853a5-666e-4e6b-b110-2de6fb0f96f9             |
| disk_or_part_device_node | /dev/sda5                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part5 |
| lvm_pv_name              | /dev/sda5                                        |
| lvm_vg_name              | cgts-vg                                          |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | f94c8e04-da78-4a63-ba5b-01d793eaa59a             |
| created_at               | 2019-05-29T09:43:34.198338+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 4f30ad74-d104-49a1-b7b9-21f8d5efcda8                              |
| ihost_uuid            | f94c8e04-da78-4a63-ba5b-01d793eaa59a                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T09:43:39.245773+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
+--------------------------+--------------------------------------------------+
| Property                 | Value                                            |
+--------------------------+--------------------------------------------------+
| uuid                     | 56d855f3-5396-4646-87c6-971c9c4b355e             |
| pv_state                 | adding                                           |
| pv_type                  | partition                                        |
| disk_or_part_uuid        | b123ed8d-b1f5-4f30-a4e8-30ca8e263fc3             |
| disk_or_part_device_node | /dev/sda6                                        |
| disk_or_part_device_path | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0-part6 |
| lvm_pv_name              | /dev/sda6                                        |
| lvm_vg_name              | nova-local                                       |
| lvm_pv_uuid              | None                                             |
| lvm_pv_size_gib          | 0.0                                              |
| lvm_pe_total             | 0                                                |
| lvm_pe_alloced           | 0                                                |
| ihost_uuid               | f94c8e04-da78-4a63-ba5b-01d793eaa59a             |
| created_at               | 2019-05-29T09:43:41.207194+00:00                 |
| updated_at               | None                                             |
+--------------------------+--------------------------------------------------+
+-----------------------+-------------------------------------------------------------------+
| Property              | Value                                                             |
+-----------------------+-------------------------------------------------------------------+
| lvm_vg_name           | nova-local                                                        |
| vg_state              | adding                                                            |
| uuid                  | 4f30ad74-d104-49a1-b7b9-21f8d5efcda8                              |
| ihost_uuid            | f94c8e04-da78-4a63-ba5b-01d793eaa59a                              |
| lvm_vg_access         | None                                                              |
| lvm_max_lv            | 0                                                                 |
| lvm_cur_lv            | 0                                                                 |
| lvm_max_pv            | 0                                                                 |
| lvm_cur_pv            | 0                                                                 |
| lvm_vg_size_gib       | 0.0                                                               |
| lvm_vg_avail_size_gib | 0.0                                                               |
| lvm_vg_total_pe       | 0                                                                 |
| lvm_vg_free_pe        | 0                                                                 |
| created_at            | 2019-05-29T09:43:39.245773+00:00                                  |
| updated_at            | None                                                              |
| parameters            | {u'concurrent_disk_operations': 2, u'instance_backing': u'image'} |
+-----------------------+-------------------------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do                                                                                        [8/1955]
>    system host-if-modify $COMPUTE mgmt0 --networks cluster-host
> done
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:77:70:e6                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | 196ce0c6-b8a5-40ce-92e7-55bc46d0cb2f |
| ihost_uuid   | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:23:43.241306+00:00     |
| updated_at   | 2019-05-29T09:41:32.791324+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| ifname       | mgmt0                                |
| iftype       | ethernet                             |
| ports        | [u'enp2s2']                          |
| datanetworks | []                                   |
| imac         | 52:54:00:69:a0:5c                    |
| imtu         | 1500                                 |
| ifclass      | platform                             |
| networks     | cluster-host,mgmt                    |
| aemode       | None                                 |
| schedpolicy  | None                                 |
| txhashpolicy | None                                 |
| uuid         | fb2a30f1-cb75-48fc-b513-74c16181d7e1 |
| ihost_uuid   | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| vlan_id      | None                                 |
| uses         | []                                   |
| used_by      | []                                   |
| created_at   | 2019-05-29T09:24:49.252601+00:00     |
| updated_at   | 2019-05-29T09:41:42.254207+00:00     |
| sriov_numvfs | 0                                    |
| ipv4_mode    | static                               |
| ipv6_mode    | disabled                             |
| accelerated  | [False]                              |
+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ for COMPUTE in compute-0 compute-1; do
>     system host-unlock $COMPUTE
> done
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | 7c653bb5-de5d-4a50-98c3-a3dedfbad4f3 |
| config_status       | None                                 |
| config_target       | 7c653bb5-de5d-4a50-98c3-a3dedfbad4f3 |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:12:09.750282+00:00     |
| hostname            | compute-0                            |
| id                  | 3                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.8                        |
| mgmt_mac            | 52:54:00:77:70:e6                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-29T09:43:45.715089+00:00     |
| uptime              | 1250                                 |
| uuid                | b27ddfa9-9d87-4580-ad09-dcc83056195d |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
+---------------------+--------------------------------------+
| Property            | Value                                |
+---------------------+--------------------------------------+
| action              | none                                 |
| administrative      | locked                               |
| availability        | online                               |
| bm_ip               | None                                 |
| bm_type             | None                                 |
| bm_username         | None                                 |
| boot_device         | sda                                  |
| capabilities        | {}                                   |
| config_applied      | 05e2267f-8c7d-49b6-98e2-b59d2c1e893e |
| config_status       | None                                 |
| config_target       | 05e2267f-8c7d-49b6-98e2-b59d2c1e893e |
| console             | ttyS0,115200                         |
| created_at          | 2019-05-29T09:13:01.680278+00:00     |
| hostname            | compute-1                            |
| id                  | 4                                    |
| install_output      | text                                 |
| install_state       | completed                            |
| install_state_info  | None                                 |
| invprovision        | unprovisioned                        |
| location            | {}                                   |
| mgmt_ip             | 192.168.204.14                       |
| mgmt_mac            | 52:54:00:69:a0:5c                    |
| operational         | disabled                             |
| personality         | worker                               |
| reserved            | False                                |
| rootfs_device       | sda                                  |
| serialid            | None                                 |
| software_load       | 19.01                                |
| task                | Unlocking                            |
| tboot               | false                                |
| ttys_dcd            | None                                 |
| updated_at          | 2019-05-29T09:43:45.737225+00:00     |
| uptime              | 1186                                 |
| uuid                | f94c8e04-da78-4a63-ba5b-01d793eaa59a |
| vim_progress_status | None                                 |
+---------------------+--------------------------------------+
(reverse-i-search)`-list': for COMPUTE in compute-0 compute-1; do   ROOT_DISK=$(system host-show ${COMPUTE} | grep rootfs | awk '{print $4}');   ROOT_DISK_UUID=$(system host-disk-list ${COMPUTE} --nowrap | awk /${ROOT_DISK}/'{print $2}');   AVAIL_SIZE=$(system host-disk^Cist ${COMPUTE} | awk /${ROOT_DISK}/'{printf("%d",$12)}');   CGTS_PARTITION_SIZE=4;   NOVA_PARTITION_SIZE=$(($AVAIL_SIZE - $CGTS_PARTITION_SIZE));   CGTS_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${CGTS_PARTITION_SIZE});   CGTS_PARTITION_UUID=$(echo ${CGTS_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}');   NOVA_PARTITION=$(system host-disk-partition-add -t lvm_phys_vol ${COMPUTE} ${ROOT_DISK_UUID} ${NOVA_PARTITION_SIZE});   NOVA_PARTITION_UUID=$(echo ${NOVA_PARTITION} | grep -ow "| uuid | [a-z0-9\-]* |" | awk '{print $4}');   system host-lvg-add ${COMPUTE} cgts-vg;   system host-pv-add ${COMPUTE} cgts-vg  ${CGTS_PARTITION_UUID};   system host-lvg-add ${COMPUTE} nova-local;   system host-pv-add ${COMPUTE} nova-local ${NOVA_PARTITION_UUID};   system host-lvg-modify -b image ${COMPUTE} nova-local; done
(reverse-i-search)`host': for COMPUTE in compute-0 compute-1; do     system ^Cst-unlock $COMPUTE; done
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | compute-0    | worker      | unlocked       | disabled    | offline      |
| 4  | compute-1    | worker      | unlocked       | disabled    | offline      |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | compute-0    | worker      | unlocked       | disabled    | offline      |
| 4  | compute-1    | worker      | unlocked       | disabled    | offline      |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```


```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------+-------------------------------+---------------+-----------+---------------------------------+
| application         | version | manifest name                 | manifest file | status    | progress                        |
+---------------------+---------+-------------------------------+---------------+-----------+---------------------------------+
| platform-integ-apps | 1.0-5   | platform-integration-manifest | manifest.yaml | uploading | validating and uploading charts |
+---------------------+---------+-------------------------------+---------------+-----------+---------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------+-------------------------------+---------------+----------+-----------+
| application         | version | manifest name                 | manifest file | status   | progress  |
+---------------------+---------+-------------------------------+---------------+----------+-----------+
| platform-integ-apps | 1.0-5   | platform-integration-manifest | manifest.yaml | uploaded | completed |
+---------------------+---------+-------------------------------+---------------+----------+-----------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ ls
ansible.log  localhost.yml  stx-openstack-1.0-13-centos-stable-latest.tgz
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-upload stx-openstack-1.0-13-centos-stable-latest.tgz
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------------------------+-------------------------------+---------------+-----------+---------------------------------+
| application         | version                   | manifest name                 | manifest file | status    | progress                        |
+---------------------+---------------------------+-------------------------------+---------------+-----------+---------------------------------+
| platform-integ-apps | 1.0-5                     | platform-integration-manifest | manifest.yaml | applying  | completed                       |
| stx-openstack       | 1.0-13-centos-stable-     | armada-manifest               | manifest.yaml | uploading | validating and uploading charts |
|                     | latest                    |                               |               |           |                                 |
|                     |                           |                               |               |           |                                 |
+---------------------+---------------------------+-------------------------------+---------------+-----------+---------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------------------------+-------------------------------+---------------+--------------+--------------------------------------+
| application         | version                   | manifest name                 | manifest file | status       | progress                             |
+---------------------+---------------------------+-------------------------------+---------------+--------------+--------------------------------------+
| platform-integ-apps | 1.0-5                     | platform-integration-manifest | manifest.yaml | apply-failed | operation aborted, check logs for    |
|                     |                           |                               |               |              | detail                               |
|                     |                           |                               |               |              |                                      |
| stx-openstack       | 1.0-13-centos-stable-     | armada-manifest               | manifest.yaml | uploaded     | completed                            |
|                     | latest                    |                               |               |              |                                      |
|                     |                           |                               |               |              |                                      |
+---------------------+---------------------------+-------------------------------+---------------+--------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------------------------+-------------------------------+-----------+--------------+----------------------------------------------------+
| application         | version                   | manifest name                 | manifest  | status       | progress                                           |
|                     |                           |                               | file      |              |                                                    |
+---------------------+---------------------------+-------------------------------+-----------+--------------+----------------------------------------------------+
| platform-integ-apps | 1.0-5                     | platform-integration-manifest | manifest. | apply-failed | operation aborted, check logs for detail           |
|                     |                           |                               | yaml      |              |                                                    |
|                     |                           |                               |           |              |                                                    |
| stx-openstack       | 1.0-13-centos-stable-     | armada-manifest               | manifest. | apply-failed | platform-integ-apps is required and is not applied |
|                     | latest                    |                               | yaml      |              |                                                    |
|                     |                           |                               |           |              |                                                    |
+---------------------+---------------------------+-------------------------------+-----------+--------------+----------------------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-remove stx-openstack
+---------------+----------------------------------+
| Property      | Value                            |
+---------------+----------------------------------+
| active        | False                            |
| app_version   | 1.0-13-centos-stable-latest      |
| created_at    | 2019-05-29T10:19:35.835738+00:00 |
| manifest_file | manifest.yaml                    |
| manifest_name | armada-manifest                  |
| name          | stx-openstack                    |
| progress      | None                             |
| status        | removing                         |
| updated_at    | 2019-05-29T10:26:40.763072+00:00 |
+---------------+----------------------------------+
Please use 'system application-list' or 'system application-show stx-openstack' to view the current progress.
[wrsroot@controller-0 ~(keystone_admin)]$ system application-delete stx-openstack
Application stx-openstack deleted.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-remove platform-integ-apps
+---------------+----------------------------------+
| Property      | Value                            |
+---------------+----------------------------------+
| active        | False                            |
| app_version   | 1.0-5                            |
| created_at    | 2019-05-29T10:16:46.752035+00:00 |
| manifest_file | manifest.yaml                    |
| manifest_name | platform-integration-manifest    |
| name          | platform-integ-apps              |
| progress      | None                             |
| status        | removing                         |
| updated_at    | 2019-05-29T10:22:03.525403+00:00 |
+---------------+----------------------------------+
Please use 'system application-list' or 'system application-show platform-integ-apps' to view the current progress.
[wrsroot@controller-0 ~(keystone_admin)]$ system application-delete platform-integ-apps
Application platform-integ-apps deleted.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system application-list
+---------------------+---------+-------------------------------+---------------+-----------+---------------------------------+
| application         | version | manifest name                 | manifest file | status    | progress                        |
+---------------------+---------+-------------------------------+---------------+-----------+---------------------------------+
| platform-integ-apps | 1.0-5   | platform-integration-manifest | manifest.yaml | uploading | validating and uploading charts |
+---------------------+---------+-------------------------------+---------------+-----------+---------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-list                                                                                                                      
+----+--------------+-------------+----------------+-------------+--------------+
| id | hostname     | personality | administrative | operational | availability |
+----+--------------+-------------+----------------+-------------+--------------+
| 1  | controller-0 | controller  | unlocked       | enabled     | available    |
| 2  | controller-1 | controller  | unlocked       | enabled     | available    |
| 3  | compute-0    | worker      | unlocked       | disabled    | failed       |
| 4  | compute-1    | worker      | unlocked       | disabled    | failed       |
| 5  | storage-0    | storage     | unlocked       | enabled     | available    |
| 6  | storage-1    | storage     | unlocked       | enabled     | available    |
+----+--------------+-------------+----------------+-------------+--------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock compute-0
[wrsroot@controller-0 ~(keystone_admin)]$ system host-lock compute-1
```

- https://www.google.com/search?client=ubuntu&channel=fs&q=%22operation+aborted%2C+check+logs+for+detail%22&ie=utf-8&oe=utf-8

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pods --all-namespaces | grep tiller
kube-system   tiller-deploy-9c6df7f79-7x8fp              1/1     Running   1          3h39m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get services --all-namespaces | grep tiller
kube-system   tiller-deploy   ClusterIP   10.109.71.107   <none>        44134/TCP       3h39m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get deployments.apps --all-namespaces | grep tiller
kube-system   tiller-deploy             1/1     1            1           3h39m
[wrsroot@controller-0 ~(keystone_admin)]$ get pods --all-namespaces | grep tiller
-sh: get: command not found
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pods --all-namespaces | grep tiller
kube-system   tiller-deploy-9c6df7f79-7x8fp              1/1     Running   1          3h40m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get services --all-namespaces | grep tiller
kube-system   tiller-deploy   ClusterIP   10.109.71.107   <none>        44134/TCP       3h40m
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get deployments.apps --all-namespaces | grep tiller
kube-system   tiller-deploy             1/1     1            1           3h40m
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ cat /etc/build.info
###
### StarlingX
###     Built from master
###

OS="centos"
SW_VERSION="19.01"
BUILD_TARGET="Host Installer"
BUILD_TYPE="Formal"
BUILD_ID="20190528T202529Z"

JOB="STX_build_master_master"
BUILD_BY="starlingx.build@cengn.ca"
BUILD_NUMBER="119"
BUILD_HOST="starlingx_mirror"
BUILD_DATE="2019-05-28 20:25:29 +0000"
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ cat /var/log/sysinv.log
...
...
2019-05-29 12:15:47.743 95438 INFO sysinv.conductor.kube_app [-] Application overrides generated.
2019-05-29 12:15:47.781 95438 INFO sysinv.conductor.kube_app [-] Armada manifest file has no img tags for chart helm-toolkit
2019-05-29 12:15:47.804 95438 INFO sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/quay.io/external_storage/rbd-provisioner:v2.1.1-k8s1.11 download started from local registry
2019-05-29 12:15:47.858 95438 INFO sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/docker.io/port/ceph-config-helper:v1.10.3 download started from local registry
2019-05-29 12:15:58.047 95438 ERROR sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/quay.io/external_storage/rbd-provisioner:v2.1.1-k8s1.11 download failed from local registry: 500 Server Error: Internal Server Error ("Get https://192.168.204.2:9001/v2/: net/http: TLS handshake timeout")
2019-05-29 12:15:58.089 95438 ERROR sysinv.conductor.kube_app [-] Image 192.168.204.2:9001/docker.io/port/ceph-config-helper:v1.10.3 download failed from local registry: 500 Server Error: Internal Server Error ("Get https://192.168.204.2:9001/v2/: net/http: TLS handshake timeout")
2019-05-29 12:15:58.090 95438 ERROR sysinv.conductor.kube_app [-] Deployment of application platform-integ-apps (1.0-5) failed: failed to download one or more image(s).
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app Traceback (most recent call last):
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app   File "/usr/lib64/python2.7/site-packages/sysinv/conductor/kube_app.py", line 1212, in perform_app_apply
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app     self._download_images(app)
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app   File "/usr/lib64/python2.7/site-packages/sysinv/conductor/kube_app.py", line 546, in _download_images
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app     reason="failed to download one or more image(s).")
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app KubeAppApplyFailure: Deployment of application platform-integ-apps (1.0-5) failed: failed to download one or more image(s).
2019-05-29 12:15:58.090 95438 TRACE sysinv.conductor.kube_app 
2019-05-29 12:15:58.100 95438 ERROR sysinv.conductor.kube_app [-] Application apply aborted!.
2019-05-29 12:16:25.598 96284 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_start_2019-05-29-12-16-25 patch
2019-05-29 12:16:25.599 96284 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:16:27.045 96284 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_start_2019-05-29-12-16-27 patch
2019-05-29 12:16:27.046 96284 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:16:53.927 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_start_2019-05-29-12-16-53 patch
2019-05-29 12:16:53.927 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:18:41.853 96283 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_start_2019-05-29-12-18-41 patch
2019-05-29 12:18:41.853 96283 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:19:41.285 96283 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_start_2019-05-29-12-19-41 patch
2019-05-29 12:19:41.285 96283 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:20:07.508 96284 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_start_2019-05-29-12-20-07 patch
2019-05-29 12:20:07.508 96284 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:21:25.620 96283 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_start_2019-05-29-12-21-25 patch
2019-05-29 12:21:25.620 96283 INFO sysinv.api.controllers.v1.host [-] storage-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:21:27.079 96283 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_start_2019-05-29-12-21-27 patch
2019-05-29 12:21:27.079 96283 INFO sysinv.api.controllers.v1.host [-] storage-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:21:54.012 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_start_2019-05-29-12-21-53 patch
2019-05-29 12:21:54.013 96284 INFO sysinv.api.controllers.v1.host [-] compute-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:23:41.921 96284 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_start_2019-05-29-12-23-41 patch
2019-05-29 12:23:41.922 96284 INFO sysinv.api.controllers.v1.host [-] controller-1 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:24:41.586 96284 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_start_2019-05-29-12-24-41 patch
2019-05-29 12:24:41.586 96284 INFO sysinv.api.controllers.v1.host [-] controller-0 ihost_patch_end.  No changes from mtce/1.0.
2019-05-29 12:25:07.515 96283 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_start_2019-05-29-12-25-07 patch
2019-05-29 12:25:07.515 96283 INFO sysinv.api.controllers.v1.host [-] compute-1 ihost_patch_end.  No changes from mtce/1.0.
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker logs armada_service
Password: 
+ CMD=armada
+ ARMADA_UWSGI_PORT=8000
+ ARMADA_UWSGI_TIMEOUT=3600
+ ARMADA_UWSGI_WORKERS=4
+ ARMADA_UWSGI_THREADS=1
+ '[' server = server ']'
+ exec uwsgi -b 32768 --die-on-term --http :8000 --http-timeout 3600 --enable-threads -L --lazy-apps --master --paste config:/etc/armada/api-paste.ini --pyargv '--config-file $
etc/armada/armada.conf' --threads 1 --workers 4
*** Starting uWSGI 2.0.18 (64bit) on [Wed May 29 09:06:44 2019] ***
compiled with version: 7.3.0 on 09 April 2019 13:34:27
os: Linux-3.10.0-957.1.3.el7.1.tis.x86_64 #1 SMP PREEMPT Tue May 28 21:28:13 UTC 2019
nodename: d2887dc5d97e
machine: x86_64
clock source: unix
detected number of CPU cores: 4
current working directory: /armada
detected binary path: /usr/local/bin/uwsgi
!!! no internal routing support, rebuild with pcre support !!!
your memory page size is 4096 bytes
detected max file descriptor number: 65536
lock engine: pthread robust mutexes
thunder lock: disabled (you can enable it with --thunder-lock)
uWSGI http bound on :8000 fd 4
uwsgi socket 0 bound to TCP address 127.0.0.1:46633 (port auto-assigned) fd 3
Python version: 3.6.7 (default, Oct 22 2018, 11:32:17)  [GCC 8.2.0]
Python main interpreter initialized at 0x5571f22eda70
python threads support enabled
your server socket listen backlog is limited to 100 connections                                                                                                         [0/1999]
your mercy for graceful operations on workers is 60 seconds
mapped 507960 bytes (496 KB) for 4 cores
*** Operational MODE: preforking ***
*** uWSGI is running in multiple interpreter mode ***
spawned uWSGI master process (pid: 1)
spawned uWSGI worker 1 (pid: 15, cores: 1)
spawned uWSGI worker 2 (pid: 16, cores: 1)
spawned uWSGI worker 3 (pid: 17, cores: 1)
spawned uWSGI worker 4 (pid: 18, cores: 1)
spawned uWSGI http 1 (pid: 19)
Loading paste environment: config:/etc/armada/api-paste.ini
Loading paste environment: config:/etc/armada/api-paste.ini
Loading paste environment: config:/etc/armada/api-paste.ini
Loading paste environment: config:/etc/armada/api-paste.ini
2019-05-29 09:06:45.997 15 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:45.998 15 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:45.998 15 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0x5571f22eda70 pid: 15 (default app)
2019-05-29 09:06:45.998 16 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:45.999 16 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:45.999 16 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 1 seconds on interpreter 0x5571f22eda70 pid: 16 (default app)
2019-05-29 09:06:46.009 17 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:46.009 17 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:46.009 17 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 2 seconds on interpreter 0x5571f22eda70 pid: 17 (default app)
2019-05-29 09:06:46.018 18 WARNING keystonemiddleware.auth_token [-] Use of the auth_admin_prefix, auth_host, auth_port, auth_protocol, identity_uri, admin_token, admin_user, a
dmin_password, and admin_tenant_name configuration options was deprecated in the Mitaka release in favor of an auth_plugin and its related options. This class may be removed in
 a future release.
2019-05-29 09:06:46.018 18 WARNING keystonemiddleware.auth_token [-] Configuring admin URI using auth fragments was deprecated in the Kilo release, and will be removed in the N
 release, use 'identity_uri\ instead.
2019-05-29 09:06:46.018 18 WARNING keystonemiddleware.auth_token [-] Configuring auth_uri to point to the public identity endpoint is required; clients may not be able to authe
nticate against an admin endpoint
WSGI app 0 (mountpoint='') ready in 2 seconds on interpreter 0x5571f22eda70 pid: 18 (default app)
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service sh -c 'ls *.log'
platform-integ-apps-delete.log
stx-openstack-delete.log
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service sh -c 'cat stx-openstack-delete.log'
2019-05-29 10:27:23.891 129 DEBUG armada.handlers.tiller [-] Using Tiller namespace: kube-system _get_tiller_namespace /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:174
2019-05-29 10:27:23.906 129 DEBUG armada.handlers.tiller [-] Found at least one Running Tiller pod. _get_tiller_pod /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:150
2019-05-29 10:27:23.907 129 DEBUG armada.handlers.tiller [-] Using Tiller pod IP: 192.168.204.3 _get_tiller_ip /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:165
2019-05-29 10:27:23.907 129 DEBUG armada.handlers.tiller [-] Using Tiller host port: 44134 _get_tiller_port /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:170
2019-05-29 10:27:23.907 129 DEBUG armada.handlers.tiller [-] Tiller getting gRPC insecure channel at 192.168.204.3:44134 with options: [grpc.max_send_message_length=429496729, grpc.max_receive_message_length=429496729] get_channel /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:124
2019-05-29 10:27:23.910 129 DEBUG armada.handlers.tiller [-] Armada is using Tiller at: None:44134, namespace=kube-system, timeout=300 __init__ /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:104
2019-05-29 10:27:23.916 129 INFO armada.handlers.lock [-] Acquiring lock
2019-05-29 10:27:23.929 129 DEBUG armada.handlers.tiller [-] Getting known releases from Tiller... list_charts /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:349
2019-05-29 10:27:23.929 129 DEBUG armada.handlers.tiller [-] Tiller ListReleases() with timeout=300, request=limit: 32
status_codes: UNKNOWN
status_codes: DEPLOYED
status_codes: DELETED
status_codes: DELETING
status_codes: FAILED
status_codes: PENDING_INSTALL
status_codes: PENDING_UPGRADE
status_codes: PENDING_ROLLBACK
 get_results /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:210
2019-05-29 10:27:24.225 129 INFO armada.cli [-] There's no release to delete.
2019-05-29 10:27:24.930 129 INFO armada.handlers.lock [-] Releasing lock

```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ sudo docker exec armada_service sh -c 'cat platform-integ-apps-delete.log'
2019-05-29 12:12:12.332 245 DEBUG armada.handlers.tiller [-] Using Tiller namespace: kube-system _get_tiller_namespace /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:174
2019-05-29 12:12:12.345 245 DEBUG armada.handlers.tiller [-] Found at least one Running Tiller pod. _get_tiller_pod /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:150
2019-05-29 12:12:12.345 245 DEBUG armada.handlers.tiller [-] Using Tiller pod IP: 192.168.204.3 _get_tiller_ip /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:165
2019-05-29 12:12:12.346 245 DEBUG armada.handlers.tiller [-] Using Tiller host port: 44134 _get_tiller_port /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:170
2019-05-29 12:12:12.346 245 DEBUG armada.handlers.tiller [-] Tiller getting gRPC insecure channel at 192.168.204.3:44134 with options: [grpc.max_send_message_length=429496729, grpc.max_receive_message_length=429496729] get_channel /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:124
2019-05-29 12:12:12.348 245 DEBUG armada.handlers.tiller [-] Armada is using Tiller at: None:44134, namespace=kube-system, timeout=300 __init__ /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:104
2019-05-29 12:12:12.353 245 INFO armada.handlers.lock [-] Acquiring lock
2019-05-29 12:12:12.362 245 DEBUG armada.handlers.tiller [-] Getting known releases from Tiller... list_charts /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:349
2019-05-29 12:12:12.362 245 DEBUG armada.handlers.tiller [-] Tiller ListReleases() with timeout=300, request=limit: 32
status_codes: UNKNOWN
status_codes: DEPLOYED
status_codes: DELETED
status_codes: DELETING
status_codes: FAILED
status_codes: PENDING_INSTALL
status_codes: PENDING_UPGRADE
status_codes: PENDING_ROLLBACK
 get_results /usr/local/lib/python3.6/dist-packages/armada/handlers/tiller.py:210
2019-05-29 12:12:12.376 245 INFO armada.cli [-] There's no release to delete.
2019-05-29 12:12:13.364 245 INFO armada.handlers.lock [-] Releasing lock
```


# Bare Metal

```sh
Release 19.01 localhost ttyS0
------------------------------------------------------------------------
W A R N I N G *** W A R N I N G *** W A R N I N G *** W A R N I N G *** 
------------------------------------------------------------------------
THIS IS A PRIVATE COMPUTER SYSTEM.
This computer system including all related equipment, network devices
(specifically including Internet access), are provided only for authorized use.
All computer systems may be monitored for all lawful purposes, including to
ensure that their use is authorized, for management of the system, to
facilitate protection against unauthorized access, and to verify security
procedures, survivability and operational security. Monitoring includes active
attacks by authorized personnel and their entities to test or verify the
security of the system. During monitoring, information may be examined,
recorded, copied and used for authorized purposes. All information including
personal information, placed on or sent over this system may be monitored. Uses
of this system, authorized or unauthorized, constitutes consent to monitoring
of this system. Unauthorized use may subject you to criminal prosecution.
Evidence of any such unauthorized use collected during monitoring may be used
for administrative, criminal or other adverse action. Use of this system
constitutes consent to monitoring for these purposes.

localhost login: wrsroot
Password: 
You are required to change your password immediately (root enforced)
Changing password for wrsroot.
(current) UNIX password: 
New password: 
Retype new password: 

WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

localhost:~$ 
```

```
localhost:~$ sudo config_controller --force --config-file config.ini                                                                                                            
Parsing system configuration file...  DONE
Validating system configuration file...  DONE
Creating config apply file...  DONE

The following configuration will be applied:

System Configuration
--------------------
Time Zone: UTC
System mode: duplex
Distributed Cloud System Controller: no

PXEBoot Network Configuration
-----------------------------
Separate PXEBoot network not configured
PXEBoot Controller floating hostname: pxecontroller

Management Network Configuration
--------------------------------
Management interface name: eno2
Management interface: eno2
Management interface MTU: 1500
Management subnet: 10.10.58.0/24
Controller floating address: 10.10.58.2
Controller 0 address: 10.10.58.3
Controller 1 address: 10.10.58.4
NFS Management Address 1: 10.10.58.5
NFS Management Address 2: 10.10.58.6
Controller floating hostname: controller
Controller hostname prefix: controller-
OAM Controller floating hostname: oamcontroller
Dynamic IP address allocation is selected
Management multicast subnet: 239.1.1.0/28

Kubernetes Cluster Network Configuration
----------------------------------------
Cluster pod network subnet: 172.16.0.0/16
Cluster service network subnet: 10.96.0.0/12
Cluster host interface name: eno2
Cluster host interface: eno2
Cluster host interface MTU: 1500
Cluster host subnet: 192.168.206.0/24

External OAM Network Configuration
----------------------------------
External OAM interface name: eno1
External OAM interface: eno1
External OAM interface MTU: 1500
External OAM subnet: 192.168.200.0/24
External OAM gateway address: 192.168.200.1
External OAM floating address: 192.168.200.201
External OAM 0 address: 192.168.200.73
External OAM 1 address: 192.168.200.86

DNS Configuration
-----------------
Nameserver 1: 192.168.100.60

Docker Registry Configuration
-----------------------------
Alternative registry to k8s.gcr.io: 192.168.100.60
Alternative registry to gcr.io: 192.168.100.60
Alternative registry to quay.io: 192.168.100.60
Alternative registry to docker.io: 192.168.100.60
Is registries secure: False

Applying configuration (this will take several minutes):

01/08: Creating bootstrap configuration ... DONE
02/08: Applying bootstrap manifest ... DONE
03/08: Persisting local configuration ... DONE
04/08: Populating initial system inventory ... DONE
05/08: Creating system configuration ... 2019-05-29 11:51:41,730 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:41.730 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:42,065 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:42.065 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:42,095 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:42.095 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:42,096 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:42.096 76505 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,413 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.413 76522 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,734 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.734 76522 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,764 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.764 76522 WARNING ceph_client [-] skip checking server certificate
2019-05-29 11:51:44,766 WARNING ceph_client skip checking server certificate
sysinv 2019-05-29 11:51:44.766 76522 WARNING ceph_client [-] skip checking server certificate
DONE
06/08: Applying controller manifest ... 
Failed to execute controller manifest

Configuration failed: Failed to apply controller manifest. See /var/log/puppet/latest/puppet.log for details.
localhost:~$ 
```

Issues

```sh
06/08: Applying controller manifest ... 
[ 1115.133420] block drbd2: Unrelated data, aborting!
[ 1116.194033] block drbd1: Unrelated data, aborting!
[ 1116.274835] block drbd5: Unrelated data, aborting!
[ 1118.807825] block drbd3: Unrelated data, aborting!
[ 1119.807887] block drbd0: Unrelated data, aborting!
[ 1134.299383] block drbd5: Unrelated data, aborting!
[ 1136.749724] block drbd2: Unrelated data, aborting!
[ 1139.486247] block drbd1: Unrelated data, aborting!
[ 1141.610208] block drbd8: Unrelated data, aborting!
[ 1141.615197] block drbd3: Unrelated data, aborting!
```

