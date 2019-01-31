# Packet Bare Metal

# StarlingX ISO

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

```
Type "exit" to return to menu.                                                  
iPXE> kernel https://boot.netboot.xyz/memdisk iso raw                           
https://boot.netboot.xyz/memdisk... ok                                          
```

When pasting full Starling ISO URL:
http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190130T060000Z/outputs/iso/bootimage.iso
Only N number of characters were accepted, they got truncated at a specific length:
_http://mirror.starlingx.cengn.ca/mirror/starlingx/ma_

```
iPXE> initrd http://mirror.starlingx.cengn.ca/mirror/starlingx/ma               
http://mirror.starlingx.cengn.ca/mirror/starlingx/ma... No such file or director
y (http://ipxe.org/2d0c613b)
```

To fix, a specific length of the string was pasted, then manually some characters were typed until a new line given, so fianlly the rest of the charecters were pasted:

```
iPXE> initrd http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/201
90130T060000Z/outputs/iso/bootimage.iso                                         
http://mirror.starlingx.cengn.ca/mirror/starlingx/master/centos/20190130T060000Z
/outputs/iso/bootimage.iso... 86%                    
```

When StarlingX ISO was loaded, and after 

```
iPXE> initrd http://mirror.starlingx.cengn.ca/mirror/starlingx/master/cento     
e820: 000000007f136000 000000000026f000 4                                       
MEMDISK: Image appears to be truncated                                      rw  
                                                                                
/outputs/iso/bootimage.iso... ok                                                
iPXE> boot                          
```

# iPXE Demo

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
