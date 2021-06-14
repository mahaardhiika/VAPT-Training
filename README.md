# VAPT Course Cheat Sheet
This Repository is intended to support VAPT Course. All cheats we need in the course is lay down here. Enjoy!
-----------

## Linux Basic





## Scanning
### NMAP Command
| # | Options |Description |
| --- | --- | --- |
|1| -sT | TCP Connect port scan|
|2| -sU | UDP Port scan |
|3| -p  | specific port scan|
|4| -p- | Scan All ports|
|5| -sV | Check Version of running service |
|6| -sC | Scan with default safe Scripts |
|7| -O | OS Fingerprinting |

| # | Command | Scan For |
| --- | --- | --- |
|1| nmap -sV `Target_IP` | Open TCP Ports and versions |
|2| nmap -sT `Target IP` | Open TCP Ports |
|3| nmap -sU `Target IP` | Open UDP Ports |
|4| nmap -sC -sV -p- `Target IP` | Open All TCP Ports and Versions + Scan with default NSE Scripts |
|5| nmap --vuln `Target IP` | Scan for vulnerability () |

### Search for NMAP Scripts
```
ls /usr/share/nmap/scripts
```

### Interesting ports
| Port # | Description |
| --- | --- |
| 21 | FTP server, unencrypted. |
| 22 | SSH server, can be connected to via SSH |
| 23 | Telnet. Basically an unencrypted SSH |
| 25 | SMTP - Email sending service. [Query it](#SMTP-Email-Enumeration) to enum email addresses? |
| 69 | TFTP Server.  Very uncommon and old. Uses UDP. |
| 80 | HTTP Server, hosting website? Try visiting IP with web browser |
| 88 | Kerberos Service.  Check, [MS14-068](https://labs.f-secure.com/archive/digging-into-ms14-068-exploitation-and-defence/) |
| 110 | POP3 mail service.  Login via telnet or SSH? |
| 111 | RPCbind. This can help us look for NFS-shares |
| 119 | Network Time Protocol |
| 135 | MSRPC - Microsoft RPC |
| 139 | SMB Service. likely vulnerable to an SMB RCE |
| 161, 162 | SNMP Service |
| 389, 636 | LDAP Directory Service |
| 443 | HTTPS, check for HeartBleed? View certificate for information? |
| 445 | SMB Shares service, likely vulnerable to an SMB RCE |
| 587 | Submission.  If Postfix is run on it, it could be vunerable to shellshock |
| 631 | CUPS. Basically a Linux Printer Service for sharing printers. |
| 1433 | Default MSSQL port.  `sqsh -S 10.1.11.41 -U sa` |
| 1521 | Oracle DB. `tnscmd10g version -h 10.1.11.51` |
| 2021 | Oracle XML DB.  Check [Default Passwords](https://docs.oracle.com/cd/B10501_01/win.920/a95490/username.htm) |
| 2049 | Network File System. `showmount -e 10.1.11.64` |
| 3306 | MySQL Database.  Connect: `mysql --host=10.1.11.69 -u root -p`|
| 3389 | Listening for RDP connection |

## Enumeration
### FTP
Check for Anonymous login allowed
```
ftp open `Target IP`
```
> Username: anonymous ; password sembarang
### SMB



### Search For Public Exploit
Exploit-DB, Google, Github

Searchsploit
```
searchsploit `exploit keyword` \\Mencari exploit dengan keyword tertentu


```
