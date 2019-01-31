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

```
user@workstation:~/starlingx/documentation/latest/stx-update$ ssh *.packet.net
The authenticity of host 'sos.dfw2.packet.net (0.0.0.0)' can't be established.
RSA key fingerprint is SHA256:*.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '*.packet.net,0.0.0.0' (RSA) to the list of known hosts.
[Read-only SOS Session. Use ~? for help.]
~?
Supported escape sequences:
 ~.   - terminate connection (and any multiplexed sessions)
 ~B   - send a BREAK to the remote system
 ~C   - open a command line
 ~R   - request rekey
 ~V/v - decrease/increase verbosity (LogLevel)
 ~^Z  - suspend ssh
 ~#   - list forwarded connections
 ~&   - background ssh (when waiting for connections to terminate)
 ~?   - this message
 ~~   - send the escape character by typing it twice
(Note that escapes are only recognized immediately after newline.)
```
