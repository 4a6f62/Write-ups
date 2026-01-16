Initial nmap:
![[images/Pasted image 20260105110103.png]]

Hydra on ssh and trying to connect via ssh:
![[images/Pasted image 20260105110403.png]]

Netcat on port 25:
![[images/Pasted image 20260105110517.png]]

Nmap on all ports:
![[images/Pasted image 20260105110643.png]]
Found port 60000

netcat on port 6000:
![[images/Pasted image 20260105110723.png]]
This is also a ssh port?

Also cant connect on 6000 via ssh
![[images/Pasted image 20260105110837.png]]

Curl on port 80 reveils bin message:
![[images/Pasted image 20260105110915.png]]

Bin to ASCII reveils taunting message:
![[images/Pasted image 20260105111002.png]]

nxc reveils no shares:
![[images/Pasted image 20260105111143.png]]

nmap on all open ports with -sCV, reveils Sendmail version:
```ssh
Nmap scan report for 192.168.130.42
Host is up, received echo-reply ttl 61 (0.019s latency).
Scanned at 2026-01-05 11:13:01 CET for 41s

PORT      STATE SERVICE     REASON         VERSION
22/tcp    open  ssh         syn-ack ttl 61 OpenSSH 3.8.1p1 Debian 8.sarge.6 (protocol 2.0)
| ssh-hostkey: 
|   1024 30:3e:a4:13:5f:9a:32:c0:8e:46:eb:26:b3:5e:ee:6d (DSA)
| ssh-dss AAAAB3NzaC1kc3MAAACBALr/RyBq802QXa1Bh4SQEUHqD+p9TEx3SUvPHACbT0tQqR3aali+ifDiOpqMToVaRfWzYOOsoM2Neg0EPa4KsJIwSTkFqjd/3Ynp3Yzus0nN+gtmbQRKzo8QfStr6IGt6kaI6viXl4z3ww6ryEkjNnb74KCooHOjyeGPi3o89GVnAAAAFQDSg0dwMrSn9juW/XPvo8S8kVOhDQAAAIARaqFuvZCqiTY8i/PITsr5WvyZm8mQ0nuqB6gW6y1h4jDAvtHO4TIZEMJ5vtPst0w9mVSYGVFlukhCqhbJdBigqH1WB1p7kwC78M9k23zZmzuwbnzYPiLHpEdfFEWdO62ZoCSFBXWOqe1IZaTaRCgUZPeB1QFXRCQ96VrJizPLUAAAAIEArOALxR78fZrUqmUcYOs5tf8wu5xChAUqAfh1ElJ6r3EjcWwXId12jo1uAz0JmCTluUQhjhNDJB6XIgUzoFzW1NZPjGCkex7s1+2+TUTmqFr6Nr97k2RIy91Bpuxwg5jzE83cKPCOoWVbYlfzAqNkF4xxznfC3fRtmj2e/L9chzg=
|   1024 af:a2:49:3e:d8:f2:26:12:4a:a0:b5:ee:62:76:b0:18 (RSA)
|_ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAIEAviGcDkDxKzv7w++DXy6q+5AJDpG/q8Um8j4BheW9fgwsOvQCuDvLcPUIKMYEz4aUgkt/sSCXu29XTlu79pEkb48+BnaRCKrHLH/YWM79GT6Q5ie9jP47HjjJeCCBI/c02qpkH/fjz9FK4HQPC7WtXY9EgW4IMB+pzX2KZxK2PF0=
25/tcp    open  smtp        syn-ack ttl 61 Sendmail 8.13.4/8.13.4/Debian-3sarge3
| smtp-commands: localhost.localdomain Hello [192.168.45.181], pleased to meet you, ENHANCEDSTATUSCODES, PIPELINING, EXPN, VERB, 8BITMIME, SIZE, DSN, ETRN, DELIVERBY, HELP
|_ 2.0.0 This is sendmail version 8.13.4 2.0.0 Topics: 2.0.0 HELO EHLO MAIL RCPT DATA 2.0.0 RSET NOOP QUIT HELP VRFY 2.0.0 EXPN VERB ETRN DSN AUTH 2.0.0 STARTTLS 2.0.0 For more info use "HELP <topic>". 2.0.0 To report bugs in the implementation send email to 2.0.0 sendmail-bugs@sendmail.org. 2.0.0 For local information send email to Postmaster at your site. 2.0.0 End of HELP info
80/tcp    open  http        syn-ack ttl 61 Apache httpd 1.3.33 ((Debian GNU/Linux))
| http-methods: 
|   Supported Methods: GET HEAD OPTIONS TRACE
|_  Potentially risky methods: TRACE
|_http-title: Ph33r
|_http-server-header: Apache/1.3.33 (Debian GNU/Linux)
139/tcp   open  netbios-ssn syn-ack ttl 61 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
199/tcp   open  smux        syn-ack ttl 61 Linux SNMP multiplexer
445/tcp   open  netbios-ssn syn-ack ttl 61 Samba smbd 3.0.14a-Debian (workgroup: WORKGROUP)
60000/tcp open  ssh         syn-ack ttl 61 OpenSSH 3.8.1p1 Debian 8.sarge.6 (protocol 2.0)
| ssh-hostkey: 
|   1024 30:3e:a4:13:5f:9a:32:c0:8e:46:eb:26:b3:5e:ee:6d (DSA)
| ssh-dss AAAAB3NzaC1kc3MAAACBALr/RyBq802QXa1Bh4SQEUHqD+p9TEx3SUvPHACbT0tQqR3aali+ifDiOpqMToVaRfWzYOOsoM2Neg0EPa4KsJIwSTkFqjd/3Ynp3Yzus0nN+gtmbQRKzo8QfStr6IGt6kaI6viXl4z3ww6ryEkjNnb74KCooHOjyeGPi3o89GVnAAAAFQDSg0dwMrSn9juW/XPvo8S8kVOhDQAAAIARaqFuvZCqiTY8i/PITsr5WvyZm8mQ0nuqB6gW6y1h4jDAvtHO4TIZEMJ5vtPst0w9mVSYGVFlukhCqhbJdBigqH1WB1p7kwC78M9k23zZmzuwbnzYPiLHpEdfFEWdO62ZoCSFBXWOqe1IZaTaRCgUZPeB1QFXRCQ96VrJizPLUAAAAIEArOALxR78fZrUqmUcYOs5tf8wu5xChAUqAfh1ElJ6r3EjcWwXId12jo1uAz0JmCTluUQhjhNDJB6XIgUzoFzW1NZPjGCkex7s1+2+TUTmqFr6Nr97k2RIy91Bpuxwg5jzE83cKPCOoWVbYlfzAqNkF4xxznfC3fRtmj2e/L9chzg=
|   1024 af:a2:49:3e:d8:f2:26:12:4a:a0:b5:ee:62:76:b0:18 (RSA)
|_ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAIEAviGcDkDxKzv7w++DXy6q+5AJDpG/q8Um8j4BheW9fgwsOvQCuDvLcPUIKMYEz4aUgkt/sSCXu29XTlu79pEkb48+BnaRCKrHLH/YWM79GT6Q5ie9jP47HjjJeCCBI/c02qpkH/fjz9FK4HQPC7WtXY9EgW4IMB+pzX2KZxK2PF0=
Service Info: Host: localhost.localdomain; OSs: Linux, Unix; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.14a-Debian)
|   NetBIOS computer name: 
|   Workgroup: WORKGROUP\x00
|_  System time: 2026-01-05T10:13:08-05:00
|_smb2-time: Protocol negotiation failed (SMB2)
|_clock-skew: mean: 7h29m55s, deviation: 3h32m07s, median: 4h59m55s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 18349/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 30468/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 49857/udp): CLEAN (Failed to receive data)
|   Check 4 (port 13420/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_smb2-security-mode: Couldn't establish a SMBv2 connection.
| smb-security-mode: 
|   account_used: guest
|   authentication_level: share (dangerous)
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| nbstat: NetBIOS name: 0XBABE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   0XBABE<00>           Flags: <unique><active>
|   0XBABE<03>           Flags: <unique><active>
|   0XBABE<20>           Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
| Statistics:
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 11:13
Completed NSE at 11:13, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 11:13
Completed NSE at 11:13, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 11:13
Completed NSE at 11:13, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 42.03 seconds
           Raw packets sent: 11 (460B) | Rcvd: 8 (336B)
```

feroxbuster on port 80 reveils no new info:
![[images/Pasted image 20260105111647.png]]

Searchsploit on vuln version of Sendmail returns 2051.py
![[images/Pasted image 20260105112017.png]]
Moved 2051 to current dir:
![[images/Pasted image 20260105112126.png]]

Edited 2051 to match IP-address of target:
![[images/Pasted image 20260105112247.png]]
This exploit doesn't return anything usefull.

This wasn't the right exploit, after looking up a walkthrough I found, this, but I'm not sure how I would have been able to deduce this other then the box name being ClamAV....
![[Pasted image 20260105134330.png]]

![[images/Pasted image 20260105125453.png]]

download 4761, added chmod +x and exucte, returns error, used perl to run, now it works:
![[images/Pasted image 20260105124937.png]]

Nmap op port 31337 now reveils new port: 
![[images/Pasted image 20260105124956.png]]

netcat to port 31337, who is returns root, cd to /root, found proof.txt.
![[images/Pasted image 20260105125210.png]]