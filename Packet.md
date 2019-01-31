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

## Output

Using __Out-of-Band Console__

```
Type "exit" to return to menu.                                                  
iPXE> kernel https://boot.netboot.xyz/memdisk iso raw                           
https://boot.netboot.xyz/memdisk... ok                                          
```

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

Unresponsive console for me, it seems same behaviour with StarlingX meaning _Image appears to be truncated_ is not meaningful, what is next?

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
