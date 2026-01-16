### Nmap
## [Scanning Options](https://academy.hackthebox.com/beta/module/19/section/99#scanning-options)

|**Nmap Option**|**Description**|
|---|---|
|`10.10.10.0/24`|Target network range.|
|`-sn`|Disables port scanning.|
|`-Pn`|Disables ICMP Echo Requests|
|`-n`|Disables DNS Resolution.|
|`-PE`|Performs the ping scan by using ICMP Echo Requests against the target.|
|`--packet-trace`|Shows all packets sent and received.|
|`--reason`|Displays the reason for a specific result.|
|`--disable-arp-ping`|Disables ARP Ping Requests.|
|`--top-ports=<num>`|Scans the specified top ports that have been defined as most frequent.|
|`-p-`|Scan all ports.|
|`-p22-110`|Scan all ports between 22 and 110.|
|`-p22,25`|Scans only the specified ports 22 and 25.|
|`-F`|Scans top 100 ports.|
|`-sS`|Performs an TCP SYN-Scan.|
|`-sA`|Performs an TCP ACK-Scan.|
|`-sU`|Performs an UDP Scan.|
|`-sV`|Scans the discovered services for their versions.|
|`-sC`|Perform a Script Scan with scripts that are categorized as "default".|
|`--script <script>`|Performs a Script Scan by using the specified scripts.|
|`-O`|Performs an OS Detection Scan to determine the OS of the target.|
|`-A`|Performs OS Detection, Service Detection, and traceroute scans.|
|`-D RND:5`|Sets the number of random Decoys that will be used to scan the target.|
|`-e`|Specifies the network interface that is used for the scan.|
|`-S 10.10.10.200`|Specifies the source IP address for the scan.|
|`-g`|Specifies the source port for the scan.|
|`--dns-server <ns>`|DNS resolution is performed by using a specified name server.|

## [Output Options](https://academy.hackthebox.com/beta/module/19/section/99#output-options)

|**Nmap Option**|**Description**|
|---|---|
|`-oA filename`|Stores the results in all available formats starting with the name of "filename".|
|`-oN filename`|Stores the results in normal format with the name "filename".|
|`-oG filename`|Stores the results in "grepable" format with the name of "filename".|
|`-oX filename`|Stores the results in XML format with the name of "filename".|

## [Performance Options](https://academy.hackthebox.com/beta/module/19/section/99#performance-options)

| **Nmap Option**              | **Description**                                              |
| ---------------------------- | ------------------------------------------------------------ |
| `--max-retries <num>`        | Sets the number of retries for scans of specific ports.      |
| `--stats-every=5s`           | Displays scan's status every 5 seconds.                      |
| `-v/-vv`                     | Displays verbose output during the scan.                     |
| `--initial-rtt-timeout 50ms` | Sets the specified time value as initial RTT timeout.        |
| `--max-rtt-timeout 100ms`    | Sets the specified time value as maximum RTT timeout.        |
| `--min-rate 300`             | Sets the number of packets that will be sent simultaneously. |
| `-T <0-5>`                   | Specifies the specific timing template.                      |

---

## Footprinting
## Infrastructure-based Enumeration

|**Command**|**Description**|
|---|---|
|`curl -s https://crt.sh/\?q\=<target-domain>\&output\=json \| jq .`|Certificate transparency.|
|`for i in $(cat ip-addresses.txt);do shodan host $i;done`|Scan each IP address in a list using Shodan.|

---

## Host-based Enumeration

##### FTP

|**Command**|**Description**|
|---|---|
|`ftp <FQDN/IP>`|Interact with the FTP service on the target.|
|`nc -nv <FQDN/IP> 21`|Interact with the FTP service on the target.|
|`telnet <FQDN/IP> 21`|Interact with the FTP service on the target.|
|`openssl s_client -connect <FQDN/IP>:21 -starttls ftp`|Interact with the FTP service on the target using encrypted connection.|
|`wget -m --no-passive ftp://anonymous:anonymous@<target>`|Download all available files on the target FTP server.|

##### SMB

|**Command**|**Description**|
|---|---|
|`smbclient -N -L //<FQDN/IP>`|Null session authentication on SMB.|
|`smbclient //<FQDN/IP>/<share>`|Connect to a specific SMB share.|
|`rpcclient -U "" <FQDN/IP>`|Interaction with the target using RPC.|
|`samrdump.py <FQDN/IP>`|Username enumeration using Impacket scripts.|
|`smbmap -H <FQDN/IP>`|Enumerating SMB shares.|
|`crackmapexec smb <FQDN/IP> --shares -u '' -p ''`|Enumerating SMB shares using null session authentication.|
|`enum4linux-ng.py <FQDN/IP> -A`|SMB enumeration using enum4linux.|

##### NFS

|**Command**|**Description**|
|---|---|
|`showmount -e <FQDN/IP>`|Show available NFS shares.|
|`mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock`|Mount the specific NFS share to ./target-NFS|
|`umount ./target-NFS`|Unmount the specific NFS share.|

##### DNS

|**Command**|**Description**|
|---|---|
|`dig ns <domain.tld> @<nameserver>`|NS request to the specific nameserver.|
|`dig any <domain.tld> @<nameserver>`|ANY request to the specific nameserver.|
|`dig axfr <domain.tld> @<nameserver>`|AXFR request to the specific nameserver.|
|`dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>`|Subdomain brute forcing.|

##### SMTP

|**Command**|**Description**|
|---|---|
|`telnet <FQDN/IP> 25`||

##### IMAP/POP3

|**Command**|**Description**|
|---|---|
|`curl -k 'imaps://<FQDN/IP>' --user <user>:<password>`|Log in to the IMAPS service using cURL.|
|`openssl s_client -connect <FQDN/IP>:imaps`|Connect to the IMAPS service.|
|`openssl s_client -connect <FQDN/IP>:pop3s`|Connect to the POP3s service.|

##### SNMP

|**Command**|**Description**|
|---|---|
|`snmpwalk -v2c -c <community string> <FQDN/IP>`|Querying OIDs using snmpwalk.|
|`onesixtyone -c community-strings.list <FQDN/IP>`|Bruteforcing community strings of the SNMP service.|
|`braa <community string>@<FQDN/IP>:.1.*`|Bruteforcing SNMP service OIDs.|

##### MySQL

|**Command**|**Description**|
|---|---|
|`mysql -u <user> -p<password> -h <FQDN/IP>`|Login to the MySQL server.|

##### MSSQL

|**Command**|**Description**|
|---|---|
|`mssqlclient.py <user>@<FQDN/IP> -windows-auth`|Log in to the MSSQL server using Windows authentication.|

##### IPMI

|**Command**|**Description**|
|---|---|
|`msf6 auxiliary(scanner/ipmi/ipmi_version)`|IPMI version detection.|
|`msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)`|Dump IPMI hashes.|

##### Linux Remote Management

|**Command**|**Description**|
|---|---|
|`ssh-audit.py <FQDN/IP>`|Remote security audit against the target SSH service.|
|`ssh <user>@<FQDN/IP>`|Log in to the SSH server using the SSH client.|
|`ssh -i private.key <user>@<FQDN/IP>`|Log in to the SSH server using private key.|
|`ssh <user>@<FQDN/IP> -o PreferredAuthentications=password`|Enforce password-based authentication.|

##### Windows Remote Management

|**Command**|**Description**|
|---|---|
|`rdp-sec-check.pl <FQDN/IP>`|Check the security settings of the RDP service.|
|`xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>`|Log in to the RDP server from Linux.|
|`evil-winrm -i <FQDN/IP> -u <user> -p <password>`|Log in to the WinRM server.|
|`wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"`|Execute command using the WMI service.|

##### Oracle TNS

|**Command**|**Description**|
|---|---|
|`./odat.py all -s <FQDN/IP>`|Perform a variety of scans to gather information about the Oracle database services and its components.|
|`sqlplus <user>/<pass>@<FQDN/IP>/<db>`|Log in to the Oracle database.|
|`./odat.py utlfile -s <FQDN/IP> -d <db> -U <user> -P <pass> --sysdba --putFile C:\\insert\\path file.txt ./file.txt`|Upload a file with Oracle RDBMS.|