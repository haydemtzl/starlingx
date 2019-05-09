# Provisioning controller-0

## Set the ntp server

```sh
WARNING: Unauthorized access to this system is forbidden and will be
prosecuted by law. By accessing this system, you agree that your
actions may be monitored if unauthorized usage is suspected.

(reverse-i-search)`': ^C
controller-0:~$ source /etc/platform/openrc 
[wrsroot@controller-0 ~(keystone_admin)]$ 
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system ntp-modify ntpservers=0.pool.ntp.org,1.pool.ntp.org
+--------------+--------------------------------------+
| Property     | Value                                |
+--------------+--------------------------------------+
| uuid         | f220ebcc-874d-4d47-91cd-5c922b83b416 |
| enabled      | True                                 |
| ntpservers   | 0.pool.ntp.org,1.pool.ntp.org        |
| isystem_uuid | 57b9f9a3-5840-4a6d-89fe-e4c6aa956b47 |
| created_at   | 2019-05-09T07:57:52.887066+00:00     |
| updated_at   | None                                 |
+--------------+--------------------------------------+
```

## Configure the vswitch type (optional)

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system modify --vswitch_type ovs-dpdk
+----------------------+--------------------------------------+
| Property             | Value                                |
+----------------------+--------------------------------------+
| contact              | None                                 |
| created_at           | 2019-05-09T07:57:52.857427+00:00     |
| description          | None                                 |
| https_enabled        | False                                |
| location             | None                                 |
| name                 | 5873e93f-e035-4d22-8c2e-7dacf7c78003 |
| region_name          | RegionOne                            |
| sdn_enabled          | False                                |
| security_feature     | spectre_meltdown_v1                  |
| service_project_name | services                             |
| software_version     | 19.01                                |
| system_mode          | duplex                               |
| system_type          | Standard                             |
| timezone             | UTC                                  |
| updated_at           | 2019-05-09T07:58:41.681534+00:00     |
| uuid                 | 57b9f9a3-5840-4a6d-89fe-e4c6aa956b47 |
| vswitch_type         | ovs-dpdk                             |
+----------------------+--------------------------------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-cpu-modify -f vswitch -p0 1 controller-0
Can only modify worker node cores.
```

## Prepare the host for running the containerized services

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ source /etc/platform/openrc
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ system host-label-assign controller-0 openstack-control-plane=enabled
+-------------+--------------------------------------+
| Property    | Value                                |
+-------------+--------------------------------------+
| uuid        | 41f98a54-2058-4f2e-ae0b-cb8b303fac9b |
| host_uuid   | dd87579c-9945-4ae1-8de0-ce8cb3622eeb |
| label_key   | openstack-control-plane              |
| label_value | enabled                              |
+-------------+--------------------------------------+
```

## Unlock controller-0

```sh
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
| config_applied      | 6cb8ff6a-bf51-4998-89ea-f17b4e369b76       |
| config_status       | Config out-of-date                         |
| config_target       | ecb8ff6a-bf51-4998-89ea-f17b4e369b76       |
| console             | tty0                                       |
| created_at          | 2019-05-09T07:58:44.594232+00:00           |
| hostname            | controller-0                               |
| id                  | 1                                          |
| install_output      | text                                       |
| install_state       | None                                       |
| install_state_info  | None                                       |
| invprovision        | provisioned                                |
| location            | {}                                         |
| mgmt_ip             | 192.168.204.3                              |
| mgmt_mac            | 52:54:00:ca:3f:93                          |
| operational         | disabled                                   |
| personality         | controller                                 |
| reserved            | False                                      |
| rootfs_device       | /dev/disk/by-path/pci-0000:00:1f.2-ata-1.0 |
| serialid            | None                                       |
| software_load       | 19.01                                      |
| task                | Unlocking                                  |
| tboot               | false                                      |
| ttys_dcd            | None                                       |
| updated_at          | 2019-05-09T08:22:55.842887+00:00           |
| uptime              | 8602                                       |
| uuid                | dd87579c-9945-4ae1-8de0-ce8cb3622eeb       |
| vim_progress_status | None                                       |
+---------------------+--------------------------------------------+
```

