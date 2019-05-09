````sh
[wrsroot@controller-0 ~(keystone_admin)]$ system service-list

+-----+----------------------------+--------------+----------------+
| id  | service_name               | hostname     | state          |
+-----+----------------------------+--------------+----------------+
| 102 | barbican-api               | controller-0 | enabled-active |
| 103 | barbican-keystone-listener | controller-0 | enabled-active |
| 104 | barbican-worker            | controller-0 | enabled-active |
| 54  | ceph-manager               | controller-0 | enabled-active |
| 67  | ceph-radosgw               | controller-0 | enabled-active |
| 14  | cgcs-export-fs             | controller-0 | enabled-active |
| 10  | cgcs-fs                    | controller-0 | enabled-active |
| 16  | cgcs-nfs-ip                | controller-0 | enabled-active |
| 105 | cluster-host-ip            | controller-0 | enabled-active |
| 23  | dnsmasq                    | controller-0 | enabled-active |
| 107 | docker-distribution        | controller-0 | enabled-active |
| 108 | dockerdistribution-fs      | controller-0 | enabled-active |
| 5   | drbd-cgcs                  | controller-0 | enabled-active |
| 109 | drbd-dockerdistribution    | controller-0 | enabled-active |
| 100 | drbd-etcd                  | controller-0 | enabled-active |
| 75  | drbd-extension             | controller-0 | enabled-active |
| 3   | drbd-pg                    | controller-0 | enabled-active |
| 6   | drbd-platform              | controller-0 | enabled-active |
| 4   | drbd-rabbit                | controller-0 | enabled-active |
| 99  | etcd                       | controller-0 | enabled-active |
| 101 | etcd-fs                    | controller-0 | enabled-active |
| 77  | extension-export-fs        | controller-0 | enabled-active |
| 76  | extension-fs               | controller-0 | enabled-active |
| 24  | fm-mgr                     | controller-0 | enabled-active |
| 62  | guest-agent                | controller-0 | enabled-active |
| 64  | haproxy                    | controller-0 | enabled-active |
| 114 | helmrepository-fs          | controller-0 | enabled-active |
| 51  | horizon                    | controller-0 | enabled-active |
| 22  | hw-mon                     | controller-0 | enabled-active |
| 25  | keystone                   | controller-0 | enabled-active |
| 50  | lighttpd                   | controller-0 | enabled-active |
| 2   | management-ip              | controller-0 | enabled-active |
| 53  | mgr-restful-plugin         | controller-0 | enabled-active |
| 20  | mtc-agent                  | controller-0 | enabled-active |
| 9   | nfs-mgmt                   | controller-0 | enabled-active |
| 48  | open-ldap                  | controller-0 | enabled-active |
| 52  | patch-alarm-manager        | controller-0 | enabled-active |
| 7   | pg-fs                      | controller-0 | enabled-active |
| 15  | platform-export-fs         | controller-0 | enabled-active |
| 11  | platform-fs                | controller-0 | enabled-active |
| 17  | platform-nfs-ip            | controller-0 | enabled-active |
| 12  | postgres                   | controller-0 | enabled-active |
| 65  | pxeboot-ip                 | controller-0 | enabled-active |
| 13  | rabbit                     | controller-0 | enabled-active |
| 8   | rabbit-fs                  | controller-0 | enabled-active |
| 115 | registry-token-server      | controller-0 | enabled-active |
| 49  | snmp                       | controller-0 | enabled-active |
| 19  | sysinv-conductor           | controller-0 | enabled-active |
| 18  | sysinv-inv                 | controller-0 | enabled-active |
| 59  | vim                        | controller-0 | enabled-active |
| 60  | vim-api                    | controller-0 | enabled-active |
| 61  | vim-webserver              | controller-0 | enabled-active |
+-----+----------------------------+--------------+----------------+
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$  fm alarm-list
+----------+------------------------------------------------------------------------+----------------------------+----------+-------------------+
| Alarm ID | Reason Text                                                            | Entity ID                  | Severity | Time Stamp        |
+----------+------------------------------------------------------------------------+----------------------------+----------+-------------------+
| 100.114  | NTP address 199.223.248.99 is not a valid or a reachable NTP server.   | host=controller-0.ntp=199. | minor    | 2019-05-08T19:57: |
|          |                                                                        | 223.248.99                 |          | 48.468603         |
|          |                                                                        |                            |          |                   |
| 100.114  | NTP address 208.113.157.157 is not a valid or a reachable NTP server.  | host=controller-0.ntp=208. | minor    | 2019-05-08T19:57: |
|          |                                                                        | 113.157.157                |          | 48.465802         |
|          |                                                                        |                            |          |                   |
| 100.114  | NTP configuration does not contain any valid or reachable NTP servers. | host=controller-0.ntp      | major    | 2019-05-08T17:11: |
|          |                                                                        |                            |          | 34.212749         |
|          |                                                                        |                            |          |                   |
+----------+------------------------------------------------------------------------+----------------------------+----------+-------------------+
```

