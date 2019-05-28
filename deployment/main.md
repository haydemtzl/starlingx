```sh
user@workstation:~/stx-tools/deployment/libvirt$ sudo brctl delif stxbr1 vnet0
user@workstation:~/stx-tools/deployment/libvirt$ sudo ifconfig stxbr1 down
user@workstation:~/stx-tools/deployment/libvirt$ sudo brctl delbr stxbr1
```

```xml
<network>
	<name>default</name>
	<bridge name="stxbr1" stp="off"/>
	<forward mode="nat"/>
	<ip address="10.10.10.1" netmask="255.255.255.0">
	</ip>
</network>
```

```sh
user@workstation:~/stx-tools/deployment/libvirt$ sudo virsh net-define nat.xml
user@workstation:~/stx-tools/deployment/libvirt$ sudo virsh net-start default
Network default started
user@workstation:~/stx-tools/deployment/libvirt$ sudo brctl addif stxbr1 vnet0
user@workstation:~/stx-tools/deployment/libvirt$ sudo ip link set stxbr1 up
user@workstation:~/stx-tools/deployment/libvirt$ sudo ip link set vnet0 up
```

```sh
user@workstation:~/stx-tools/deployment/libvirt$ netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         10.219.128.1    0.0.0.0         UG        0 0          0 eno2
10.10.10.0      0.0.0.0         255.255.255.0   U         0 0          0 stxbr1
10.19.1.4       10.219.128.1    255.255.255.255 UGH       0 0          0 eno2
10.219.128.0    0.0.0.0         255.255.255.0   U         0 0          0 eno2
169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 eno2
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
172.18.0.0      0.0.0.0         255.255.0.0     U         0 0          0 br-ae3b7a0d4dc2
172.19.0.0      0.0.0.0         255.255.0.0     U         0 0          0 br-98b1c09cd0e0
172.20.0.0      0.0.0.0         255.255.0.0     U         0 0          0 br-87218e633a26
172.21.0.0      0.0.0.0         255.255.0.0     U         0 0          0 br-9ae1aec0c908
172.22.0.0      0.0.0.0         255.255.0.0     U         0 0          0 br-b139c692564d
172.23.0.0      0.0.0.0         255.255.0.0     U         0 0          0 br-e948fd8bfd77
192.168.191.0   0.0.0.0         255.255.255.0   U         0 0          0 ztqu3hivy5
192.168.192.0   0.0.0.0         255.255.255.0   U         0 0          0 ztrtasrz7u
```

```sh
localhost:~$ ifconfig enp2s1 10.10.10.10 up
localhost:~$ route add default gw 
```
