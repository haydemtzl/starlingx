# Packet Bare Metal

## StarlingX ISO

New Server: On Demand

- Pre Requisite: SSH Key
- Hostname: demo
- Location: SJC1, Sunnyvale CA
- Type: t1.small.x86
- OS: Custom iPXE
  - https://boot.netboot.xyz/
  
Resources

- [Custom iPXE](https://support.packet.com/kb/articles/custom-ipxe)
- [SOS: Serial over SSH](https://support.packet.com/kb/articles/sos-serial-over-ssh)
- [Grub & Kernels](https://support.packet.com/kb/articles/grub-kernels)

## Output

Using __Out-of-Band Console__

- x86 servers require console=ttyS1,115200n8
- aarch64 servers require console=ttyAMA0,115200

### Parameter Kernel

Option one, arguments:

```sh
Type "exit" to return to menu.                                                  
iPXE> kernel https://boot.netboot.xyz/memdisk iso raw console=ttyS1,115200n8
https://boot.netboot.xyz/memdisk... ok                                          
```

Option two, arguments with variables:

```sh
iPXE> set kernel-params console=ttyS1,115200n8
iPXE> kernel https://boot.netboot.xys/memdisk iso raw ${kernel-params}
https://boot.netboot.xys/memdisk... Error 0x3e11618e (http://ipxe.org/3e11618e)
```

Option three, no arguments:

```
Type "exit" to return to menu.                                                  
iPXE> kernel https://boot.netboot.xyz/memdisk iso raw                           
https://boot.netboot.xyz/memdisk... ok                                          
```

> Make sure to use a valid StarlingX ISO image.

When pasting full Starling ISO URL:
http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190130T060000Z/outputs/iso/bootimage.iso, only N number of characters were accepted, they got truncated at a specific length:
_http://mirror.starlingx.cengn.ca/mirror/starlingx/ma_

```
iPXE> initrd http://mirror.starlingx.cengn.ca/mirror/starlingx/ma               
http://mirror.starlingx.cengn.ca/mirror/starlingx/ma... No such file or director
y (http://ipxe.org/2d0c613b)
```

To get string the valid string accepted, a specific length of the string was pasted, then manually some characters were typed until a new line was given, so finally the rest of the charecters were able to be pasted again:

```
iPXE> initrd http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/201
90130T060000Z/outputs/iso/bootimage.iso                                         
http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190130T060000Z
/outputs/iso/bootimage.iso... 86%                    
```

When StarlingX ISO was loaded, and after commanding a _boot_, error "Image appears to be truncated" was given:

```
iPXE> initrd http://mirror.starlingx.cengn.ca/mirror/starlingx/master/cento     
e820: 000000007f136000 000000000026f000 4                                       
MEMDISK: Image appears to be truncated                                      rw  
                                                                                
/outputs/iso/bootimage.iso... ok                                                
iPXE> boot                          
```

Console was unresponsive, typing different characters including _Enter_ dit not modified console. Next step was to boot a smaller image with same server and methodology: __Alpine Standard ISO__

- Are we experiencing this issue? [Booting Linux ISOs with Memdisk and iPXE](https://www.reversengineered.com/2016/01/07/booting-linux-isos-with-memdisk-and-ipxe/)
-  Do we need special modifications to StarlingX ISO image?

## Alpine ISO

```sh
iPXE> initrd http://dl-cdn.alpinelinux.org/alpine/v3.9/releases/x86_64/alpine-standard-3.9.0-x86_64.iso
```

```
Type "exit" to return to menu.                                                  
iPXE> kernel https://boot.netboot.xyz/memdisk iso raw                           
https://boot.netboot.xyz/memdisk... ok                                          
iPXE>                                                                           
iPXE> initrd http://dl-cdn.alpinelinux.org/alpine/v3.9/releases/x86_64/alpine-st
andard-3.9.0-x86_64.iso                                                         
http://dl-cdn.alpinelinux.org/alpine/v3.9/releases/x86_64/alpine-standard-3.9.0-
iPXE> boot
```

```
e820: 00000000fed01000 0000000000003000 2                                       
e820: 00000000fed08000 0000000000001000 2                                       �
e820: 00000000fed08000 0000000000001000 2                                       
e820: 00000000fed0c000 0000000000004000 2                                       
e820: 00000000fed1c000 0000000000001000 2                                       
e820: 00000000fef00000 0000000000100000 2                                       
e820: 00000000ff800000 0000000000800000 2                                       
Ramdisk at 0x78199000, length 0x06f00000                                        
command line: iso raw                                                           
MEMDISK: Image seems to have fractional end cylinder                              
MEMDISK: Image appears to be truncated                                          
Disk is hd96, 28416 K, C/H/S = 65535/255/15 (El Torito/El Torito), EDD on, rw     
Using raw access to high memory                                                 
Code 1860, meminfo 360, cmdline 8, stack 512                                       
Total size needed = 2740 bytes, allocating 3K                                   
Old dos memory at 0x87000 (map says 0x8ac00), loading at 0x86400                
1588: 0xffff  15E801: 0x3c00 0x7719                                               
INT 13 08: Success, count = 1, BPT = 0000:0000                                  
Drive probing gives drive shift limit: 0xe1                                       
old: int13 = f00074cc  int15 = 8ac0074d  int1e = f000efc7                       
new: int13 = 8640000a  int15 = 864003fd  int1e = f000efc7                         
Loading boot sector... booting...                                                                                                                                 

boot:                                                                           
```

Unresponsive console for me, it seems same behaviour with StarlingX meaning _Image appears to be truncated_ is not meaningful. __Out-of-Band Console__ was closed and open again but no sucess with more console output.

```
user@workstation:~$ ssh {server-uuid}@sos.sjc1.packet.net[SOS Session Ready. Use ~? for help.]
[Note: You may need to press RETURN or Ctrl+L to get a prompt.]
```

## Set Console Option

```
:packet_x86_64
set console console=ttyS1,115200n8
```

```
e820: 0000000100000000 0000000180000000 1                                       e820: 00000000e0000000 0000000004000000 2                                       
e820: 00000000fed01000 0000000000003000 2                                       
e820: 00000000fed08000 0000000000001000 2                                       
e820: 00000000fed0c000 0000000000004000 2                                       
e820: 00000000fed1c000 0000000000001000 2                                       
e820: 00000000fef00000 0000000000100000 2                                       
e820: 00000000ff800000 0000000000800000 2                                       
Ramdisk at 0x78192000, length 0x06f07000                                        
command line: iso raw                                                           
El Torito BVD sanity check failed.                                              
El Torito boot catalog sanity check failed.                                     
MEMDISK: Image seems to have fractional end cylinder                            
Disk is hd0, 2186172 K, C/H/S = 111/64/32 (guess/guess), EDD on, rw             
Using raw access to high memory                                                 
Code 1860, meminfo 360, cmdline 8, stack 512                                    
Total size needed = 2740 bytes, allocating 3K                                   
Old dos memory at 0x87000 (map says 0x8ac00), loading at 0x86400                
1588: 0xffff  15E801: 0x3c00 0x7719                                             
INT 13 08: Success, count = 1, BPT = 0000:0000                                  
Drive probing gives drive shift limit: 0x82                                     
old: int13 = f00074cc  int15 = 8ac0074d  int1e = f000efc7                       
new: int13 = 8640000a  int15 = 864003fd  int1e = f000efc7                       
MEMDISK: bootstrap too large to load                                         
```

After some time...

```
new: int13 = 8640000a  int15 = 864003fd  int1e = f000efc7                       
Connection to sos.sjc1.packet.net closed.                                       
user@workstation:~$ 
user@workstation:~$ ssh 448c8798-3efe-4f5a-8ae9-6386f69ab8a0@sos.sjc1.packet.net[SOS Session Ready. Use ~? for help.]
[Note: You may need to press RETURN or Ctrl+L to get a prompt.]

```

## iPXE Demo

- Taken from [iPXE](http://ipxe.org/scripting)
- Using "Add optional user data" under "Additional Settings"

```
#!ipxe

dhcp
chain http://boot.ipxe.org/demo/boot.php
```

Output

```
iPXE 1.0.255+ -- Open Source Network Boot Firmware -- http://ipxe.org           
                                                                                
Features: DNS HTTP HTTPS iSCSI TFTP VLAN AoE ELF MBOOT PXE bzImage COMBOOT Menu 
PXEXT                                                                           
                                                                                
net0: 00:00:00:00:00:00 using undionly on 0000:01:00.0 (open)                   
  [Link:up, TX:0 TXE:1 RX:0 RXE:0]                                              
  [TXE: 1 x "Network unreachable (http://ipxe.org/28086011)"]                   
Configuring (net0 00:00:00:00:00:00).................. ok                       
net0: 147.75.70.179/255.255.255.254 gw 147.75.70.178                            
net0: fe80::ec4:7aff:feb2:19b0/64                                               
Next server: 147.75.200.3                                                       
Filename: http://147.75.200.3/auto.ipxe                                         
http://147.75.200.3/auto.ipxe... ok                                             
auto.ipxe : 363 bytes [script]                                                  
Packet.net Baremetal - iPXE boot                                                
http://147.75.200.3/phone-home...... ok                                         
http://boot.ipxe.org/demo/boot.php.... ok                                       
vmlinuz-3.16.0-rc4... ok                                                        
initrd.img... 40%                                                              
```

# Errors

```sh

iPXE> kernel https://boot.netboot.xyz/memdisk iso raw console=ttyS1,115200n8
https://boot.netboot.xyz/memdisk... ok
Could not select: Exec format error (http://ipxe.org/2e008081)
```

```sh
iPXE> set kernel-params console=ttyS1,115200n8
iPXE> kernel https://boot.netboot.xys/memdisk iso raw ${kernel-params}
https://boot.netboot.xys/memdisk... Error 0x3e11618e (http://ipxe.org/3e11618e)
```

```sh
iPXE> kernel http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190206T060000Z/outputs/installer/vmlinuz iso raw ${kernel-params}
http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190206T060000Z/outputs/installer/vmlinuz... ok 
```

- http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190206T060000Z/outputs/installer/

```sh
iPXE> kernel http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/201
90206T060000Z/outputs/installer/vmlinuz ${params}                               
http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190206T060000Z
/outputs/installer/vmlinuz... ok
```

```sh
iPXE> imgstat
vmlinuz : 6587824 bytes [bzImage] [SELECTED] "console=ttyS1,115200n8"
```

```sh
iPXE> initrd http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/201
90206T060000Z/outputs/installer/initrd.img                                      
/outputs/installer/initrd.img... 46%
```

```sh
iPXE> boot
```

```sh
[    9.840044] systemd[1]: No hostname configured.
[    9.844594] systemd[1]: Set hostname to <localhost>.
[    9.849592] random: systemd: uninitialized urandom read (16 bytes read)
[    9.856216] systemd[1]: Initializing machine ID from random generator.
[    9.865044] random: systemd-cryptse: uninitialized urandom read (16 bytes read)
[    9.880254] random: systemd: uninitializeread (16 bytes read)
[    9.883144] hid-generic 0003:0557:2419.0001: input: USB HID v1.00 Keyboard [HID 0557:2419] on usb-0000:00:14.0-13.1/input0
[    9.885682] input: HID 0557:2419 as /devices/pci0000:00/0000:00:14.0/usb1/1-13/1-13.1/1-13.1:1.1/input/input4
[    9.885798] hid-generic 0003:0557:2419.0002: input: USB HID v1.00 Mouse [HID 0557:2419] on usb-0000:00:14.0-13.1/input1
[    9.918627] random: systemd: uninitialized urandom read (16 bytes read)
[    9.925260] random: systemd: uninitialized urandom read (16 bytes read)
[    9.931937] random: systemd: uninitialized urandom read (16 bytes read)
[    9.939059] random: systemd: uninitialized urandom read (16 bytes read)
[    9.948450] systemd[1]: Cannot add dependency job for unit blk-availability.service, ignoring: Unit not found.
[  OK  ] Reached target Local File Systems.
[    9.965057] systemd[1]: Reached target Local File Systems.
[    9.970560] systemd[1]: Starting Local File Systems.
[  OK  ] Reached target Timers.
[    9.980012] systemd[1]: Reached target Timers.
[    9.984461] systemd[1]: Starting Timers.
[  OK  ] Reached target Encrypted Volumes.
[    9.994008] systemd[1]: Reached target Encrypted Volumes.
[    9.999411] systemd[1]: Starting Encrypted Volumes.
[  OK  ] Created slice Root Slice.
[   10.009013] systemd[1]: Created slice Root Slice.
[   10.013719] systemd[1]: Starting Root Slice.
[  OK  ] Created slice System Slice.
[   10.023006] systemd[1]: Created slice System Slice.
[   10.027899] systemd[1]: Starting System Slice.
[  OK  ] Listening on Journal Socket.
[   10.037025] systemd[1]: Listening on Journal Socket.
[   10.042009] systemd[1]: Starting Journal Socket.
[   10.047181] systemd[1]: Starting Create list of required static device nodes for the current kernel...
         Starting Create list of required st... nodes for the current kernel...
[   10.065491] systemd[1]: Starting Journal Service...
         Starting Journal Service...
[   10.075486] systemd[1]: Starting Setup Virtual Console...
         Starting Setup Virtual Console...
[   10.086563] systemd[1]: Starting Load Kernel Modules...
[   10.089725] e1000e: loading out-of-tree module taints kernel.
         Startin[   10.097800] e1000e: Intel(R) PRO/1000 Network Driver - 3.4.2.1-NAPI
[   10.105347] e1000e: Copyright(c) 1999 - 2018 Intel Corporation.
g Load Kernel Modules...
[  OK  ] Reached target[   10.116052] i40e: Intel(R) 40-10 Gigabit Ethernet Connection Network Driver - version 2.4.10
[   10.125329] i40e: Copyright(c) 2013 - 2018 Intel Corporation.
 Slices.
[   10.133029] systemd[1]: Reached target Slices.
[   10.137499] systemd[1]: Starting Slices.
[  OK  ] Listening on udev Kernel Socke[   10.145168] i40e 0000:02:00.0: fw 6.0.48442 api 1.7 nvm 6.01 0x80003496 1.1747.0
t.
[   10.154983] systemd[1]: Listening on udev Kernel Socket.
[   10.160306] systemd[1]: Starting udev Kernel Socket.
[  OK  ] Listening on udev Control Socket.
[   10.170963] systemd[1]: Listening on udev Control Socket.
[   10.176371] systemd[1]: Starting udev Control Socket.
[   10.182113] systemd[1]: Starting dracut cmdline hook...
         Starting dracut cmdline hook...
[  OK  ] Reached target Swap.
[   10.195942] systemd[1]: Reached target Swap.
[   10.200224] systemd[1]: Starting Swap.
[  OK  ] Started Journal Service.
[   10.208946] systemd[1]: Started Journal Service.
[  OK  ] Started Create list of required sta...ce nodes for the current kernel.
[  OK  ] Started Setup Virtual Console.
         Starting Create Static Device Nodes in /dev...
[  OK  ] Started Create Static Device Nodes in /dev.
[   10.366218] i40e 0000:02:00.0: MAC address: ac:1f:6b:2c:e9:ca
[   10.377702] i40e 0000:02:00.0 eth1000: NIC Link is Up, 10 Gbps Full Duplex, Flow Control: None
[   10.386307] i40e 0000:02:00.0: PTP not supported on this device
[   10.392789] i40e 0000:02:00.0: PCI-Express: Speed 8.0GT/s Width x4
[   10.398971] i40e 0000:02:00.0: PCI-Express bandwidth available for this device may be insufficient for optimal performance.
[   10.410090] i40e 0000:02:00.0: Please move the device to a different PCI-e link with more lanes and/or higher transfer rate.
[  OK  [   10.421729] i40e 0000:02:00.0: Features: PF-id[0] VFs: 64 VSIs: 66 QP: 8 RSS FD_ATR FD_SB NTUPLE CloudF DCB VxLAN NVGRE VEPA
] Started dracut cmdline hook.
         Starting dracut pre-udev hook...
[   10.450350] i40e 0000:02:00.1: fw 6.0.48442 api 1.7 nvm 6.01 0x80003496 1.1747.0
[   10.691724] i40e 0000:02:00.1: MAC address: ac:1f:6b:2c:e9:cb
[   10.702501] i40e 0000:02:00.1: PTP not supported on this device
[   10.708878] i40e 0000:02:00.1: PCI-Express: Speed 8.0GT/s Width x4
[   10.715070] i40e 0000:02:00.1: PCI-Express bandwidth available for this device may be insufficient for optimal performance.
[   10.726189] i40e 0000:02:00.1: Please move the device to a different PCI-e link with more lanes and/or higher transfer rate.
[   10.737813] i40e 0000:02:00.1: Features: PF-id[1] VFs: 64 VSIs: 66 QP: 8 RSS FD_ATR FD_SB NTUPLE CloudF DCB VxLAN NVGRE VEPA
[   10.750199] dca service started, version 1.12.1
[   10.766056] Intel(R) 10GbE PCI Express Linux Network Driver - version 5.3.7
[   10.773015] Copyright(c) 1999 - 2018 Intel Corporation.
[   10.779160] Compat-mlnx-ofed backport release: cf60532
[   10.784299] Backport based on mlnx_ofed/mlnx-ofa_kernel-4.0.git cf60532
[   10.790916] compat.git: mlnx_ofed/mlnx-ofa_kernel-4.0.git
[   10.864447] svcrdma: svcrdma is obsoleted, loading rpcrdma instead
[   10.871123] xprtrdma: xprtrdma is obsoleted, loading rpcrdma instead
[  OK  ] Started Load Kernel Modules.
         Starting Apply Kernel Variables...
[  OK  ] Started Apply Kernel Variables.
[   13.501116] floppy0: no floppy controllers found
[   13.520073] No iBFT detected.
[   13.531401] async_tx: api initialized (async)
[   13.536705] xor: automatically using best checksumming function:
[   13.551868]    avx       : 23868.000 MB/sec
[   13.579870] raid6: sse2x1   gen() 11929 MB/s
[   13.600870] raid6: sse2x2   gen() 14632 MB/s
[   13.621871] raid6: sse2x4   gen() 16972 MB/s
[   13.642870] raid6: avx2x1   gen() 23789 MB/s
[   13.663870] raid6: avx2x2   gen() 26820 MB/s
[   13.684868] raid6: avx2x4   gen() 29921 MB/s
[   13.689133] raid6: using algorithm avx2x4 gen() (29921 MB/s)
[   13.694786] raid6: using avx2x2 recovery algorithm
[   13.728238] device-mapper: uevent: version 1.0.3
[   13.732981] device-mapper: ioctl: 4.37.1-ioctl (2018-04-03) initialised: dm-devel@redhat.com
[   13.759858] device-mapper: multipath round-robin: version 1.2.0 loaded
[   13.774600] alg: No test for __sha256-mb (__intel_sha256-mb)
[   13.782701] alg: No test for __sha256-mb (mcryptd(__intel_sha256-mb))
[   13.824404] RPC: Registered named UNIX socket transport module.
[   13.830327] RPC: Registered udp transport module.
[   13.835030] RPC: Registered tcp transport module.
[   13.839734] RPC: Registered tcp NFSv4.1 backchannel transport module.
[  OK  ] Started dracut pre-udev hook.
         Starting udev Kernel Device Manager...
[  OK  ] Started udev Kernel Device Manager.
         Starting dracut pre-trigger hook...
[   13.946166] 8021q: 802.1Q VLAN Support v1.8
[  OK  ] Started dracut pre-trigger hook.
         Starting udev Coldplug all Devices...
         Mounting Configuration File System...
[  OK  ] Mounted Configuration File System.
[  OK  ] Started udev Coldplug all Devices.
         Starting Device-Mapper [   14.082621] ipmi message handler version 39.2
[   14.086068] ACPI: Video Device [GFX0] (multi-head: yes  rom: no  post: no)
Multipath Device[   14.086587] acpi device:10: registered as cooling_device13
[   14.086667] input: Video Bus as /devices/LNXSYSTM:00/device:00/PNP0A08:00/LNXVIDEO:00/input/input5
[   14.104729] i801_smbus 0000:00:1f.4: SMBus using PCI interrupt
[   14.111420] cryptd: max_cpu_qlen set to 100
 Controller...
[   14.120481] AVX2 version of gcm_enc/dec engaged.
[   14.121191] ipmi device interface
[   14.121461] mei_me 0000:00:16.0: Device doesn't have valid ME Interface
[   14.121472] mei_me 0000:00:16.1: Device doesn't have valid ME Interface
[   14.142883] AES CTR mode by8 optimization enabled
[   14.143332] ahci 0000:00:17.0: AHCI 0001.0301 32 slots 8 ports 6 Gbps 0xff impl SATA mode
[   14.143335] ahci 0000:00:17.0: flags: 64bit ncq sntf led clo only pio slum part ems deso sadm sds apst 
[   14.147558] ipmi_si IPI0001:00: ipmi_si: probing via ACPI
[   14.147572] ipmi_si IPI0001:00: [io  0x0ca2] regsize 1 spacing 1 irq 0
[   14.147573] ipmi_si: Adding ACPI-specified kcs state machine
[   14.147594] IPMI System Interface driver.
[   14.147609] ipmi_si: probing via SMBIOS
[   14.147611] ipmi_si: SMBIOS: io 0xca2 regsize 1 spacing 1 irq 0
[   14.147611] (NULL device *): SMBIOS-specified kcs state machine: duplicate
[   14.147612] ipmi_si: probing via SPMI
[   14.147613] ipmi_si: SPMI: io 0xca2 regsize 1 spacing 1 irq 0
[   14.147614] (NULL device *): SPMI-specified kcs state machine: duplicate
[   14.147615] ipmi_si: Trying ACPI-specified kcs state machine at i/o address 0xca2, slave address 0x20, irq 0
[   14.153665] iTCO_vendor_support: vendor-support=0
[   14.154646] iTCO_wdt: Intel TCO WatchDog Timer Driver v1.11
[   14.154684] iTCO_wdt: Found a Intel PCH TCO device (Version=4, TCOBASE=0x0400)
[   14.154758] iTCO_wdt: initialized. heartbeat=30 sec (nowayout=0)
[   14.158336] alg: No test for __gcm-aes-aesni (__driver-gcm-aes-aesni)
[   14.158365] alg: No test for __generic-gcm-aes-aesni (__driver-generic-gcm-aes-aesni)
[   14.200930] scsi host0: ahci
[   14.201042] scsi host1: ahci
[   14.201137] scsi host2: ahci
[   14.201231] scsi host3: ahci
[   14.201324] scsi host4: ahci
[   14.201418] scsi host5: ahci
[   14.201512] scsi host6: ahci
[   14.201605] scsi host7: ahci
[   14.201654] ata1: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d100 irq 68
[   14.201657] ata2: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d180 irq 68
[   14.201660] ata3: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d200 irq 68
[   14.201663] ata4: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d280 irq 68
[   14.201666] ata5: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d300 irq 68
[   14.201670] ata6: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d380 irq 68
[   14.201673] ata7: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d400 irq 68
[   14.201676] ata8: SATA max UDMA/133 abar m2048@0xdf21d000 port 0xdf21d480 irq 68
[   14.234923] ipmi_si IPI0001:00: The BMC does not support clearing the recv irq bit, compensating, but the BMC needs to be fixed.
         Starting Show Plymouth Boot Screen...
[   14.368212] power_meter ACPI000D:00: Found ACPI power meter.
[   14.373900] power_meter ACPI000D:00: Ignoring unsafe software power cap!
[  OK  ] Reached target System Initialization.
[  OK  ] Listening on Open-iSCSI iscsiuio Socket.
[  OK  ] Reached target Sockets.
[  OK  ] Started Device-Mapper Multipath Device Controller.
[  OK  ] Started Show Plymouth Boot Screen.
[  OK  ] Reached target Paths.
[  OK  ] Reached target Basic System.
         Starting Open-iSCSI...
[  OK  ] Started Open-iSCSI.
         Starting dracut initqueue hook...
[   14.425974] ipmi_si IPI0001:00: Found new BMC (man_id: 0x002a7c, prod_id: 0x0951, dev_id: 0x20)
[   14.434779] ipmi_si IPI0001:00: IPMI kcs interface initialized
[   14.465448] [drm] Found 128MB of eDRAM
[   14.470950] [drm] Memory usable by graphics device = 4096M
[   14.476432] [drm] VT-d active for gfx access
[   14.480704] [drm] Replacing VGA console driver
[   14.488374] Console: switching to colour dummy device 80x25
[   14.494012] [drm:i915_gem_init_stolen [i915]] *ERROR* conflict detected with stolen region: [0x000000007e000000 - 0x0000000080000000]
[   14.499002] [drm] Supports vblank timestamp caching Rev 2 (21.10.2013).
[   14.499002] [drm] Driver supports precise vblank timestamp query.
[   14.504547] [drm] Finished loading DMC firmware i915/skl_dmc_ver1_26.bin (v1.26)
[   14.505933] ata1: SATA link down (SStatus 0 SControl 300)
[   14.511929] ata6: SATA link down (SStatus 0 SControl 300)
[   14.511965] ata4: SATA link up 6.0 Gbps (SStatus 133 SControl 300)
[   14.512930] ata8: SATA link down (SStatus 0 SControl 300)
[   14.512972] ata3: SATA link down (SStatus 0 SControl 300)
[   14.513004] ata5: SATA link down (SStatus 0 SControl 300)
[   14.513438] ata4.00: ATA-10: Micron_5100_MTFDDAK240TCB,  D0MU037, max UDMA/133
[   14.513440] ata4.00: 468862128 sectors, multi 16: LBA48 NCQ (depth 31/32), AA
[   14.513626] [drm] Disabling framebuffer compression (FBC) to prevent screen flicker with VT-d enabled
[   14.515487] ata4.00: configured for UDMA/133
[   14.517889] ata2: SATA link down (SStatus 0 SControl 300)
[   14.517920] ata7: SATA link down (SStatus 0 SControl 300)
[   14.517986] scsi 3:0:0:0: Direct-Access     ATA      Micron_5100_MTFD U037 PQ: 0 ANSI: 5
[   14.582779] ata4.00: Enabling discard_zeroes_data
[   14.582793] sd 3:0:0:0: [sda] 468862128 512-byte logical blocks: (240 GB/223 GiB)
[   14.582795] sd 3:0:0:0: [sda] 4096-byte physical blocks
[   14.582849] sd 3:0:0:0: [sda] Write Protect is off
[   14.582880] sd 3:0:0:0: [sda] Write cache: enabled, read cache: enabled, doesn't support DPO or FUA
[   14.582957] ata4.00: Enabling discard_zeroes_data
[   14.583246] ata4.00: Enabling discard_zeroes_data
[   14.583312] sd 3:0:0:0: [sda] Attached SCSI disk
[   14.651466] random: fast init done
[   14.854264] [drm] failed to retrieve link info, disabling eDP
[   14.863736] [drm] Initialized i915 1.6.0 20170818 for 0000:00:02.0 on minor 0
[   15.281999] [drm] Cannot find any crtc or sizes
[   16.329707] [drm] RC6 on
```
