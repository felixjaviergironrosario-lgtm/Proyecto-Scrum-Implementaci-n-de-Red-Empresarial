```
SW#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up      
GigabitEthernet0/1     unassigned      YES unset  up                    up      
GigabitEthernet0/2     unassigned      YES unset  up                    up      
GigabitEthernet0/3     unassigned      YES unset  up                    up      
SW#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/2, Gi0/3
70   VLAN0070                         active    Gi0/0, Gi0/1
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
SW#show ip ssh
SSH Enabled - version 2.0
Authentication methods:publickey,keyboard-interactive,password
Authentication Publickey Algorithms:x509v3-ssh-rsa,ssh-rsa
Hostkey Algorithms:x509v3-ssh-rsa,ssh-rsa
Encryption Algorithms:aes128-ctr,aes192-ctr,aes256-ctr
MAC Algorithms:hmac-sha1,hmac-sha1-96
Authentication timeout: 120 secs; Authentication retries: 3
Minimum expected Diffie Hellman key size : 1024 bits
IOS Keys in SECSH format(ssh-rsa, base64 encoded): TP-self-signed-4294967295
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgQDceWBWcSP4Ds0zHWqkFRjVOnVXxqtjrnq0vND6y9ca
cTNk84WIIYN0Ssg/hNLDXXO7M3ZRranEEUe5DnSJppQ5LcKFmS+tlRSh5p+bTwkv8sHoEsNNVdDo5Zuw
Vu9mH0cWQolFmtc264r2Y7wUoPdmWFfqNH3MRGhi+mbny5S1Aw==   
```