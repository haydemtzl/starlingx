## Others

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get services -n openstack
NAME                          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                        AGE
aodh                          ClusterIP   10.99.130.210    <none>        80/TCP,443/TCP                 7m1s
aodh-api                      ClusterIP   10.102.253.11    <none>        8042/TCP                       7m1s
barbican                      ClusterIP   10.99.204.151    <none>        80/TCP,443/TCP                 22m
barbican-api                  ClusterIP   10.99.104.218    <none>        9311/TCP                       22m
ceilometer                    ClusterIP   10.105.241.28    <none>        80/TCP,443/TCP                 2m5s
cinder                        ClusterIP   10.109.240.235   <none>        80/TCP,443/TCP                 9m51s
cinder-api                    ClusterIP   10.98.239.62     <none>        8776/TCP                       9m51s
cloudformation                ClusterIP   10.106.72.46     <none>        80/TCP,443/TCP                 13m
glance                        ClusterIP   10.106.170.140   <none>        80/TCP,443/TCP                 21m
glance-api                    ClusterIP   10.106.243.38    <none>        9292/TCP                       21m
gnocchi                       ClusterIP   10.108.73.208    <none>        80/TCP,443/TCP                 5m26s
gnocchi-api                   ClusterIP   10.103.64.185    <none>        8041/TCP                       5m26s
heat                          ClusterIP   10.97.160.185    <none>        80/TCP,443/TCP                 13m
heat-api                      ClusterIP   10.98.64.161     <none>        8004/TCP                       13m
heat-cfn                      ClusterIP   10.102.40.48     <none>        8000/TCP                       13m
horizon                       ClusterIP   10.108.237.164   <none>        80/TCP,443/TCP                 11m
horizon-int                   NodePort    10.98.52.151     <none>        80:31000/TCP                   11m
ingress                       ClusterIP   10.99.185.95     <none>        80/TCP,443/TCP,18080/TCP       25m
ingress-error-pages           ClusterIP   None             <none>        80/TCP                         25m
ingress-exporter              ClusterIP   10.103.126.95    <none>        10254/TCP                      25m
keystone                      ClusterIP   10.101.229.41    <none>        80/TCP,443/TCP                 23m
keystone-api                  ClusterIP   10.109.41.93     <none>        5000/TCP                       23m
mariadb                       ClusterIP   10.102.69.71     <none>        3306/TCP                       25m
mariadb-discovery             ClusterIP   None             <none>        3306/TCP,4567/TCP              25m
mariadb-ingress-error-pages   ClusterIP   None             <none>        80/TCP                         25m
mariadb-server                ClusterIP   10.106.77.2      <none>        3306/TCP                       25m
memcached                     ClusterIP   10.102.73.230    <none>        11211/TCP                      24m
metadata                      ClusterIP   10.104.161.229   <none>        80/TCP,443/TCP                 19m
neutron                       ClusterIP   10.99.23.230     <none>        80/TCP,443/TCP                 19m
neutron-server                ClusterIP   10.97.170.104    <none>        9696/TCP                       19m
nova                          ClusterIP   10.100.226.236   <none>        80/TCP,443/TCP                 19m
nova-api                      ClusterIP   10.104.159.200   <none>        8774/TCP                       19m
nova-api-proxy                ClusterIP   10.99.153.99     <none>        8774/TCP                       19m
nova-metadata                 ClusterIP   10.98.152.111    <none>        8775/TCP                       19m
nova-novncproxy               ClusterIP   10.105.141.147   <none>        6080/TCP                       19m
novncproxy                    ClusterIP   10.99.109.60     <none>        80/TCP,443/TCP                 19m
osh-openstac-dsv-e50215       ClusterIP   None             <none>        5672/TCP,25672/TCP,15672/TCP   24m
osh-openstac-mgr-e50215       ClusterIP   10.101.251.195   <none>        80/TCP,443/TCP                 24m
panko                         ClusterIP   10.111.200.92    <none>        80/TCP,443/TCP                 3m13s
panko-api                     ClusterIP   10.107.233.52    <none>        8977/TCP                       3m13s
placement                     ClusterIP   10.106.63.152    <none>        80/TCP,443/TCP                 19m
placement-api                 ClusterIP   10.96.99.35      <none>        8778/TCP                       19m
rabbitmq                      ClusterIP   10.103.171.246   <none>        5672/TCP,25672/TCP,15672/TCP   24m
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get configmaps -n openstack
NAME                                                          DATA   AGE
aodh-bin                                                      13     7m57s
barbican-bin                                                  9      23m
ceilometer-bin                                                13     3m1s
ceph-etc                                                      1      26m
ceph-pools-bin                                                1      26m
cinder-bin                                                    19     10m
config-rbd-provisioner                                        2      26m
glance-bin                                                    15     22m
gnocchi-bin                                                   16     6m22s
heat-bin                                                      17     14m
horizon-bin                                                   6      12m
ingress-bin                                                   2      26m
ingress-conf                                                  4      26m
ingress-services-tcp                                          0      26m
ingress-services-udp                                          0      26m
keystone-bin                                                  13     24m
libvirt-bin                                                   1      20m
mariadb-bin                                                   7      26m
mariadb-etc                                                   5      26m
mariadb-services-tcp                                          1      26m
neutron-bin                                                   22     20m
nova-api-proxy-bin                                            1      20m
nova-api-proxy-etc                                            3      20m
nova-bin                                                      32     20m
openvswitch-bin                                               3      20m
osh-openstack-ingress-nginx                                   0      26m
osh-openstack-mariadb-mariadb-state                           5      25m
osh-openstack-mariadb-osh-openstack-mariadb-mariadb-ingress   0      26m
osh-openstack-memcached-memcached-bin                         1      25m
osh-openstack-rabbitmq-rabbitmq-bin                           7      25m
osh-openstack-rabbitmq-rabbitmq-etc                           2      25m
panko-bin                                                     9      4m10s
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pods -n openstack -o wide
NAME                                                 READY   STATUS      RESTARTS   AGE     IP               NODE           NOMINATED NODE   READINESS GATES
aodh-api-998c87b4-lt8zc                              1/1     Running     0          8m36s   172.16.192.100   controller-0   <none>           <none>
aodh-db-init-dvf8t                                   0/1     Completed   0          8m36s   172.16.192.101   controller-0   <none>           <none>
aodh-db-sync-tks7p                                   0/1     Completed   0          8m36s   172.16.192.116   controller-0   <none>           <none>
aodh-evaluator-5bc6698954-bpxnx                      1/1     Running     0          8m36s   172.16.192.106   controller-0   <none>           <none>
aodh-ks-endpoints-whp6q                              0/3     Completed   0          8m36s   172.16.192.115   controller-0   <none>           <none>
aodh-ks-service-rfkgb                                0/1     Completed   0          8m36s   172.16.192.73    controller-0   <none>           <none>
aodh-ks-user-nf4xw                                   0/1     Completed   0          8m36s   172.16.192.80    controller-0   <none>           <none>
aodh-listener-55c56fbc8f-w24rn                       1/1     Running     0          8m36s   172.16.192.118   controller-0   <none>           <none>
aodh-notifier-58bd5cbfff-5797k                       1/1     Running     0          8m36s   172.16.192.94    controller-0   <none>           <none>
aodh-rabbit-init-wt8lz                               0/1     Completed   0          8m35s   172.16.192.120   controller-0   <none>           <none>
barbican-api-6f979757c8-2nbxp                        1/1     Running     0          23m     172.16.192.86    controller-0   <none>           <none>
barbican-db-init-7qmg6                               0/1     Completed   0          23m     172.16.192.69    controller-0   <none>           <none>
barbican-db-sync-fdzm8                               0/1     Completed   0          23m     172.16.192.74    controller-0   <none>           <none>
barbican-ks-endpoints-zz27s                          0/3     Completed   0          23m     172.16.192.73    controller-0   <none>           <none>
barbican-ks-service-xdhvt                            0/1     Completed   0          23m     172.16.192.71    controller-0   <none>           <none>
barbican-ks-user-7bh27                               0/1     Completed   0          23m     172.16.192.75    controller-0   <none>           <none>
barbican-rabbit-init-cnxh8                           0/1     Completed   0          23m     172.16.192.77    controller-0   <none>           <none>
ceilometer-central-5d56f6f5bc-6qpkd                  1/1     Running     0          3m40s   172.16.192.109   controller-0   <none>           <none>
ceilometer-compute-ppvqc                             1/1     Running     0          3m40s   192.168.204.3    controller-0   <none>           <none>
ceilometer-db-sync-8zp5l                             0/1     Completed   0          3m40s   172.16.192.65    controller-0   <none>           <none>
ceilometer-ks-service-fv2qq                          0/1     Completed   0          3m40s   172.16.192.121   controller-0   <none>           <none>
ceilometer-ks-user-kjjjg                             0/1     Completed   0          3m40s   172.16.192.122   controller-0   <none>           <none>
ceilometer-notification-5c5f7f9f54-56425             1/1     Running     0          3m40s   172.16.192.117   controller-0   <none>           <none>
ceilometer-rabbit-init-bpf8z                         0/1     Completed   0          3m40s   172.16.192.101   controller-0   <none>           <none>
ceph-pools-audit-1557342300-ngfth                    0/1     Completed   0          10m     172.16.192.82    controller-0   <none>           <none>
ceph-pools-audit-1557342600-52cwd                    0/1     Completed   0          5m32s   172.16.192.123   controller-0   <none>           <none>
ceph-pools-audit-1557342900-f4jc8                    0/1     Completed   0          31s     172.16.192.120   controller-0   <none>           <none>
cinder-api-78f99795b5-9wg6n                          1/1     Running     0          11m     172.16.192.98    controller-0   <none>           <none>
cinder-backup-669856cbcb-8gnhc                       1/1     Running     0          11m     172.16.192.124   controller-0   <none>           <none>
cinder-backup-storage-init-zf6px                     0/1     Completed   0          11m     172.16.192.79    controller-0   <none>           <none>
cinder-bootstrap-tjfs9                               0/1     Completed   0          11m     172.16.192.117   controller-0   <none>           <none>
cinder-db-init-95fb8                                 0/1     Completed   0          11m     172.16.192.76    controller-0   <none>           <none>
cinder-db-sync-sgk8v                                 0/1     Completed   0          11m     172.16.192.109   controller-0   <none>           <none>
cinder-ks-endpoints-qp5cl                            0/9     Completed   0          11m     172.16.192.65    controller-0   <none>           <none>
cinder-ks-service-x2pws                              0/3     Completed   0          11m     172.16.192.119   controller-0   <none>           <none>
cinder-ks-user-l4hzl                                 0/1     Completed   0          11m     172.16.192.103   controller-0   <none>           <none>
cinder-rabbit-init-bmnfh                             0/1     Completed   0          11m     172.16.192.85    controller-0   <none>           <none>
cinder-scheduler-5979cb95c8-88tsh                    1/1     Running     0          11m     172.16.192.104   controller-0   <none>           <none>
cinder-storage-init-klxsb                            0/1     Completed   0          11m     172.16.192.72    controller-0   <none>           <none>
cinder-volume-5b5f7667c9-ch98q                       1/1     Running     0          11m     172.16.192.66    controller-0   <none>           <none>
cinder-volume-usage-audit-1557342300-bqjmn           0/1     Completed   0          10m     172.16.192.121   controller-0   <none>           <none>
cinder-volume-usage-audit-1557342600-pv6gz           0/1     Completed   0          5m32s   172.16.192.88    controller-0   <none>           <none>
cinder-volume-usage-audit-1557342900-mzcwc           0/1     Completed   0          31s     172.16.192.73    controller-0   <none>           <none>
glance-api-55c46b4b7f-hv9gg                          1/1     Running     0          22m     172.16.192.111   controller-0   <none>           <none>
glance-db-init-w7qdx                                 0/1     Completed   0          22m     172.16.192.90    controller-0   <none>           <none>
glance-db-sync-4kkbk                                 0/1     Completed   0          22m     172.16.192.123   controller-0   <none>           <none>
glance-ks-endpoints-jgbbv                            0/3     Completed   0          22m     172.16.192.104   controller-0   <none>           <none>
glance-ks-service-ghxhg                              0/1     Completed   0          22m     172.16.192.125   controller-0   <none>           <none>
glance-ks-user-8ddfh                                 0/1     Completed   0          22m     172.16.192.79    controller-0   <none>           <none>
glance-rabbit-init-mzgpm                             0/1     Completed   0          22m     172.16.192.108   controller-0   <none>           <none>
glance-storage-init-l2mj6                            0/1     Completed   0          22m     172.16.192.65    controller-0   <none>           <none>
gnocchi-api-697d9ccc46-t6hsg                         1/1     Running     0          7m1s    172.16.192.108   controller-0   <none>           <none>
gnocchi-db-init-pw7jv                                0/1     Completed   0          7m1s    172.16.192.84    controller-0   <none>           <none>
gnocchi-db-sync-qbgxj                                0/1     Completed   0          7m1s    172.16.192.74    controller-0   <none>           <none>
gnocchi-ks-endpoints-crwxn                           0/3     Completed   0          7m1s    172.16.192.71    controller-0   <none>           <none>
gnocchi-ks-service-j9z9w                             0/1     Completed   0          7m1s    172.16.192.89    controller-0   <none>           <none>
gnocchi-ks-user-d82sg                                0/1     Completed   0          7m1s    172.16.192.77    controller-0   <none>           <none>
gnocchi-metricd-9h2bz                                1/1     Running     0          7m1s    172.16.192.75    controller-0   <none>           <none>
gnocchi-storage-init-58xck                           0/1     Completed   0          7m1s    172.16.192.69    controller-0   <none>           <none>
heat-api-869b69d8b4-4cb26                            1/1     Running     0          14m     172.16.192.97    controller-0   <none>           <none>
heat-bootstrap-xsn8v                                 0/1     Completed   0          14m     172.16.192.89    controller-0   <none>           <none>
heat-cfn-867858f558-dczlb                            1/1     Running     0          14m     172.16.192.91    controller-0   <none>           <none>
heat-db-init-5248s                                   0/1     Completed   0          14m     172.16.192.73    controller-0   <none>           <none>
heat-db-sync-zh27j                                   0/1     Completed   0          14m     172.16.192.69    controller-0   <none>           <none>
heat-domain-ks-user-qn96z                            0/1     Completed   0          14m     172.16.192.80    controller-0   <none>           <none>
heat-engine-6cf47797b-l5plz                          1/1     Running     0          14m     172.16.192.92    controller-0   <none>           <none>
heat-engine-cleaner-1557342300-qf5n5                 0/1     Completed   0          10m     172.16.192.122   controller-0   <none>           <none>
heat-engine-cleaner-1557342600-ljhk9                 0/1     Completed   0          5m32s   172.16.192.90    controller-0   <none>           <none>
heat-engine-cleaner-1557342900-bz7sg                 1/1     Running     0          31s     172.16.192.116   controller-0   <none>           <none>
heat-ks-endpoints-x6x2z                              0/6     Completed   0          14m     172.16.192.75    controller-0   <none>           <none>
heat-ks-service-bwb6h                                0/2     Completed   0          14m     172.16.192.71    controller-0   <none>           <none>
heat-ks-user-m7gp7                                   0/1     Completed   0          14m     172.16.192.74    controller-0   <none>           <none>
heat-rabbit-init-kxjzv                               0/1     Completed   0          14m     172.16.192.84    controller-0   <none>           <none>
heat-trustee-ks-user-h472d                           0/1     Completed   0          14m     172.16.192.77    controller-0   <none>           <none>
heat-trusts-5hrgh                                    0/1     Completed   0          14m     172.16.192.88    controller-0   <none>           <none>
horizon-7bf458fbb7-zg5s9                             1/1     Running     0          13m     172.16.192.125   controller-0   <none>           <none>
horizon-db-init-c8wpc                                0/1     Completed   0          13m     172.16.192.123   controller-0   <none>           <none>
horizon-db-sync-x85nc                                0/1     Completed   0          13m     172.16.192.90    controller-0   <none>           <none>
ingress-6ff8cbb99-mrnpt                              1/1     Running     0          27m     172.16.192.78    controller-0   <none>           <none>
ingress-error-pages-d48db556-cfm2x                   1/1     Running     0          27m     172.16.192.113   controller-0   <none>           <none>
keystone-api-799bcb4959-m7rtn                        1/1     Running     0          25m     172.16.192.87    controller-0   <none>           <none>
keystone-bootstrap-2qsr7                             0/1     Completed   0          25m     172.16.192.88    controller-0   <none>           <none>
keystone-credential-setup-wwg4w                      0/1     Completed   0          25m     172.16.192.115   controller-0   <none>           <none>
keystone-db-init-4sj4z                               0/1     Completed   0          25m     172.16.192.80    controller-0   <none>           <none>
keystone-db-sync-jstxj                               0/1     Completed   0          25m     172.16.192.92    controller-0   <none>           <none>
keystone-domain-manage-j64br                         0/1     Completed   0          25m     172.16.192.97    controller-0   <none>           <none>
keystone-fernet-setup-tpbdv                          0/1     Completed   0          25m     172.16.192.89    controller-0   <none>           <none>
keystone-rabbit-init-v72vx                           0/1     Completed   0          25m     172.16.192.91    controller-0   <none>           <none>
libvirt-libvirt-default-pxkvx                        1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
mariadb-ingress-5f6547cb4-2qvzq                      1/1     Running     0          26m     172.16.192.114   controller-0   <none>           <none>
mariadb-ingress-error-pages-fbfb8b56b-2tqkv          1/1     Running     0          26m     172.16.192.112   controller-0   <none>           <none>
mariadb-server-0                                     1/1     Running     0          26m     172.16.192.107   controller-0   <none>           <none>
neutron-db-init-kwj9v                                0/1     Completed   0          21m     172.16.192.72    controller-0   <none>           <none>
neutron-db-sync-5mdw8                                0/1     Completed   0          21m     172.16.192.118   controller-0   <none>           <none>
neutron-dhcp-agent-controller-0-a762cb46-42vxr       1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-ks-endpoints-l77gp                           0/3     Completed   0          21m     172.16.192.120   controller-0   <none>           <none>
neutron-ks-service-qdhdj                             0/1     Completed   0          21m     172.16.192.82    controller-0   <none>           <none>
neutron-ks-user-mn8h4                                0/1     Completed   0          21m     172.16.192.117   controller-0   <none>           <none>
neutron-l3-agent-controller-0-a762cb46-g4kn6         1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-metadata-agent-controller-0-a762cb46-wxh47   1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-ovs-agent-controller-0-a762cb46-xp8d6        1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
neutron-rabbit-init-cdw69                            0/1     Completed   0          21m     172.16.192.76    controller-0   <none>           <none>
neutron-server-7795f76bc6-p6fkt                      1/1     Running     0          21m     172.16.192.102   controller-0   <none>           <none>
neutron-sriov-agent-controller-0-a762cb46-g7494      1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
nova-api-metadata-b456bc9b7-wbbbl                    1/1     Running     1          21m     172.16.192.93    controller-0   <none>           <none>
nova-api-osapi-7bf769b779-m5cjb                      1/1     Running     0          21m     172.16.192.83    controller-0   <none>           <none>
nova-api-proxy-59c95df5b4-lq466                      1/1     Running     0          21m     172.16.192.70    controller-0   <none>           <none>
nova-bootstrap-lx8qj                                 0/1     Completed   0          21m     172.16.192.106   controller-0   <none>           <none>
nova-cell-setup-bpthm                                0/1     Completed   0          21m     172.16.192.116   controller-0   <none>           <none>
nova-compute-controller-0-9626473e-vmz4q             2/2     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
nova-conductor-7d479f4746-g9c76                      1/1     Running     0          21m     172.16.192.105   controller-0   <none>           <none>
nova-consoleauth-75bb89d84b-rfmv8                    1/1     Running     0          21m     172.16.192.95    controller-0   <none>           <none>
nova-db-init-qh5x5                                   0/3     Completed   0          21m     172.16.192.119   controller-0   <none>           <none>
nova-db-sync-vgh9n                                   0/1     Completed   0          21m     172.16.192.94    controller-0   <none>           <none>
nova-ks-endpoints-d992f                              0/3     Completed   0          21m     172.16.192.100   controller-0   <none>           <none>
nova-ks-service-qs8nr                                0/1     Completed   0          21m     172.16.192.98    controller-0   <none>           <none>
nova-ks-user-7fqzc                                   0/1     Completed   0          21m     172.16.192.121   controller-0   <none>           <none>
nova-novncproxy-79664ffdd8-vktn8                     1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
nova-placement-api-75f6866c4-nzfkz                   1/1     Running     0          21m     172.16.192.96    controller-0   <none>           <none>
nova-rabbit-init-dww7b                               0/1     Completed   0          21m     172.16.192.122   controller-0   <none>           <none>
nova-scheduler-768dcb6699-9nndq                      1/1     Running     0          21m     172.16.192.81    controller-0   <none>           <none>
nova-storage-init-9lcr8                              0/1     Completed   0          21m     172.16.192.85    controller-0   <none>           <none>
openvswitch-db-n7nh2                                 1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
openvswitch-vswitchd-z8mdb                           1/1     Running     0          21m     192.168.204.3    controller-0   <none>           <none>
osh-openstack-memcached-memcached-5f9c796767-28nqz   1/1     Running     0          26m     172.16.192.110   controller-0   <none>           <none>
osh-openstack-rabbitmq-cluster-wait-q64cx            0/1     Completed   0          25m     172.16.192.82    controller-0   <none>           <none>
osh-openstack-rabbitmq-rabbitmq-0                    1/1     Running     0          25m     172.16.192.127   controller-0   <none>           <none>
panko-api-787d9c8b69-m7qng                           1/1     Running     0          4m48s   172.16.192.76    controller-0   <none>           <none>
panko-db-init-6b6hq                                  0/1     Completed   0          4m48s   172.16.192.85    controller-0   <none>           <none>
panko-db-sync-8tl8n                                  0/1     Completed   0          4m48s   172.16.192.72    controller-0   <none>           <none>
panko-ks-endpoints-kv9p5                             0/3     Completed   0          4m48s   172.16.192.79    controller-0   <none>           <none>
panko-ks-service-588nw                               0/1     Completed   0          4m48s   172.16.192.119   controller-0   <none>           <none>
panko-ks-user-8zszn                                  0/1     Completed   0          4m48s   172.16.192.82    controller-0   <none>           <none>
placement-ks-endpoints-nmrnz                         0/3     Completed   0          21m     172.16.192.101   controller-0   <none>           <none>
placement-ks-service-nqxgd                           0/1     Completed   0          21m     172.16.192.124   controller-0   <none>           <none>
placement-ks-user-k5q9d                              0/1     Completed   0          21m     172.16.192.66    controller-0   <none>           <none>
rbd-provisioner-569784655f-bdt66                     1/1     Running     0          26m     172.16.192.99    controller-0   <none>           <none>
rbd-provisioner-storage-init-d996n                   0/1     Completed   0          26m     172.16.192.120   controller-0   <none>           <none>
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pvc --all-namespaces
NAMESPACE   NAME                                              STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
openstack   mysql-data-mariadb-server-0                       Bound    pvc-e4a64525-71c1-11e9-a337-5254000cf013   5Gi        RWO            general        30m
openstack   rabbitmq-data-osh-openstack-rabbitmq-rabbitmq-0   Bound    pvc-0adb5131-71c2-11e9-a337-5254000cf013   1Gi        RWO            general        29m
```

```sh
[wrsroot@controller-0 ~(keystone_admin)]$ kubectl get pv --all-namespaces
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                                       STORAGECLASS   REASON   AGE
pvc-0adb5131-71c2-11e9-a337-5254000cf013   1Gi        RWO            Delete           Bound    openstack/rabbitmq-data-osh-openstack-rabbitmq-rabbitmq-0   general                 30m
pvc-e4a64525-71c1-11e9-a337-5254000cf013   5Gi        RWO            Delete           Bound    openstack/mysql-data-mariadb-server-0                       general                 31m
```

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

