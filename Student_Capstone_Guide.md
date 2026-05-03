**SELAH TECH ACADEMY CAPSTONE PROJECT!** 🎉

You're about to embark on an exciting journey that will take you from curious bootcamp student to someone who has actually conducted a real penetration test. By the end of this week, you'll have skills and a portfolio piece that makes you marketable to employers.


let's get the hacking started
---

## TABLE OF CONTENTS

1. [Before You Start](#before-you-start)
2. [What You'll Learn](#what-youll-learn)
3. [Your Week at a Glance](#your-week-at-a-glance)
4. [Daily Breakdowns (Day-by-Day)](#daily-breakdowns)
5. [Phase Guides](#phase-guides)
6. [Tips for Success](#tips-for-success)
7. [Troubleshooting](#troubleshooting)
8. [Portfolio & Interview Prep](#portfolio--interview-prep)
9. [Resources & Cheat Sheets](#resources--cheat-sheets)

---

```bash
# Open terminal on Kali and verify:

# 1. Can you ping Metasploitable?
ping -c 2 192.168.1.101

# Expected: 2 packets sent, 2 received ✓

# 2. Can you SSH to Metasploitable?
ssh msfadmin@192.168.1.101
# Password: msfadmin
# Then type: exit

# Expected: SSH prompt appears and works ✓

# 3. Check key tools
nmap --version          # Should show version ✓
msfconsole --version    # Should show version ✓

# 4. Create your working directory
mkdir ~/capstone_project
cd ~/capstone_project
mkdir reconnaissance scanning exploitation privesc data_exfil reports

echo "Project started: $(date)" > project_log.txt
```

**All checks pass?** Great! You're ready. ✓

---

## WHAT YOU'LL LEARN

By the end of this project, you will understand:

### Technical Skills
- ✅ **Network Reconnaissance** - How to map and understand target networks
- ✅ **Vulnerability Assessment** - Finding security weaknesses using professional tools
- ✅ **Exploitation** - Turning vulnerabilities into access (the fun part!)
- ✅ **Privilege Escalation** - Going from limited access to full control
- ✅ **Post-Exploitation** - What hackers do AFTER getting in
- ✅ **Data Exfiltration** - How sensitive data gets stolen
- ✅ **Persistence** - How attackers stay in systems undetected

### Professional Skills
- ✅ **Technical Writing** - Documenting findings professionally
- ✅ **Risk Assessment** - Explaining why things matter
- ✅ **Report Creation** - Building reports for executives
- ✅ **Evidence Collection** - Proof that stands up to scrutiny
- ✅ **Time Management** - Completing complex projects on deadline

### Mindset Shift
- ✅ **Attacker Perspective** - Think like the bad guys (ethically!)
- ✅ **Problem Solving** - When A doesn't work, try B, C, D...
- ✅ **Persistence** - Keep going when things get tough
- ✅ **Learning Agility** - Quickly master new tools and techniques
- ✅ **Attention to Detail** - Catch security issues others miss

---

## YOUR WEEK AT A GLANCE

```
┌─────────────────────────────────────────────────────────┐
│          CAPSTONE PROJECT WEEK OVERVIEW                │
└─────────────────────────────────────────────────────────┘

DAY 1-2: RECONNAISSANCE ("The Investigation Phase")
├─ Learn the target network
├─ Identify services running
├─ Find potential vulnerabilities
├─ Deliverable: Reconnaissance report
└─ Time: 8-10 hours | Difficulty: ⭐⭐ (Easy)

DAY 2-3: VULNERABILITY SCANNING ("The Analysis Phase")
├─ Set up automated scanning tools
├─ Scan for detailed vulnerabilities
├─ Analyze results
├─ Prioritize targets
└─ Time: 6-8 hours | Difficulty: ⭐⭐⭐ (Medium)

DAY 3-4: EXPLOITATION ("The Fun Phase")
├─ Hack into the system
├─ Gain unauthorized access
├─ Try multiple attack vectors
├─ Document successful exploits
└─ Time: 12-16 hours | Difficulty: ⭐⭐⭐⭐ (Hard!)

DAY 4-5: PRIVILEGE ESCALATION ("The Power-Up Phase")
├─ Go from limited user to ROOT
├─ Demonstrate full control
├─ Show system compromise
├─ Prove you own the box
└─ Time: 4-6 hours | Difficulty: ⭐⭐⭐⭐ (Hard)

DAY 5-6: DATA EXFILTRATION & PERSISTENCE ("The Evidence Phase")
├─ Steal sensitive data (in lab)
├─ Show impact of breach
├─ Establish persistent access
├─ Demonstrate attacker tactics
└─ Time: 6-8 hours | Difficulty: ⭐⭐⭐ (Medium)

DAY 6-7: REPORTING ("The Professional Phase")
├─ Compile all findings
├─ Write executive summary
├─ Create technical report
├─ Build presentation slides
├─ Organize evidence
└─ Time: 8-10 hours | Difficulty: ⭐⭐⭐ (Medium)

TOTAL: 40-50 hours of hands-on cybersecurity work
RESULT: Portfolio-quality penetration test report
```

---

**Key Learning Moment:**

You just did something most people don't understand how to do. You:
1. ✓ Found a vulnerability
2. ✓ Understood what it was
3. ✓ Used a tool to exploit it
4. ✓ Gained unauthorized access
5. ✓ Verified your access

**This is real cybersecurity work.**

Questions?
```

**Save This:**
- [ ] Save as PRESENTATION.pptx
- [ ] Make slides visually appealing (add colors, diagrams)
- [ ] Practice presenting (time yourself)
- [ ] Prepare to speak for 15 minutes

---

**Evening (2 hours)**

**Final Checklist:**

- [ ] Executive Summary written & proofread
- [ ] Technical Report completed (details on each vulnerability)
- [ ] 8-slide presentation created and formatted
- [ ] All screenshots organized in folders
- [ ] Evidence files compiled
- [ ] README created explaining everything
- [ ] All work saved in ~/capstone_project/reports/

---

## TIPS FOR SUCCESS

### Time Management

**The Golden Rule:** Don't get stuck on one thing for more than 2 hours.

```
✓ Can't exploit a vulnerability? Move on, come back later.
✓ Nessus taking too long? Do other tasks while it runs.
✓ Report not perfect? Good enough is good enough.
✓ Get something done every day—momentum matters.
```
```

---

### Taking Good Screenshots

**What to Screenshot:**
- ✅ Nmap scan results
- ✅ Metasploit exploitation (before & after)
- ✅ Root access proof (whoami, id output)
- ✅ Data exfiltration
- ✅ Any interesting findings

**How to Screenshot Effectively:**
1. Take screenshot (Print Screen or Command+Shift+3)
2. Save with DESCRIPTIVE name:
   ```
   ✓ Day3_Samba_Exploitation_Success.png
   ✗ Screenshot_125.png
   ```
3. Keep in organized folder:
   ```
   ~/capstone_project/evidence/screenshots/
   ├─ reconnaissance/
   ├─ exploitation/
   ├─ privesc/
   └─ exfiltration/
   ```

**Best Practices:**
- Highlight the important part
- Include command AND output
- Show clear evidence of success
- Add arrows/circles to key findings

---

**Writing Your Report:**
- ✅ Start with what you DID (narrative)
- ✅ Add screenshots as evidence
- ✅ Explain WHY it matters
- ✅ Show business impact
- ✅ Provide solutions

**Don't:**
- ✗ Use too much technical jargon
- ✗ Assume reader knows security
- ✗ Leave findings unexplained
- ✗ Forget the business perspective

---

### Asking for Help Effectively

**When to Ask:**
- ✅ You've been stuck for 30+ minutes
- ✅ You're unsure about a concept
- ✅ You need clarification
- ✅ Something isn't working at all


## PORTFOLIO & INTERVIEW PREP

### Building Your Portfolio

**What to Show Employers:**

1. **The Executive Summary**
   - 2-3 pages showing you understand business impact
   - Professional writing
   - Clear risk assessment

2. **Key Screenshots**
   - Nessus dashboard
   - Metasploit exploitation
   - Root access proof
   - Data exfiltration

3. **The Presentation**
   - 8 slides you can present
   - Shows communication skills
   - Visual, compelling, organized

4. **Your Approach**
   - Document your methodology
   - Show problem-solving
   - Demonstrate tool knowledge

**How to Present It:**

Option 1: **GitHub Repository**
```
Create public repo:
my-capstone-project/
├─ README.md (overview)
├─ reports/
│  ├─ Executive_Summary.pdf
│  ├─ Technical_Report.pdf
│  └─ Presentation.pdf
├─ evidence/
│  └─ screenshots/
└─ tools-and-techniques.md
```

Option 2: **Portfolio Website**
```
yourname-cybersecurity.com/
├─ Capstone Project
│  ├─ Overview
│  ├─ Detailed findings
│  ├─ Screenshots
│  └─ Download PDF report
├─ Skills
└─ Contact
```

Option 3: **LinkedIn**
```
Post about the project:
"Just completed a comprehensive penetration test for 
my capstone. Identified 24 vulnerabilities, successfully 
exploited critical remote code execution, and escalated 
to root access. Full report available upon request."
```

--
## RESOURCES & CHEAT SHEETS

### Command Cheat Sheet

**Nmap:**
```bash
nmap -sV 192.168.1.101              # Service detection
nmap -A -T4 192.168.1.101           # Aggressive scan
nmap -p- 192.168.1.101              # All ports
nmap --script vuln 192.168.1.101    # Vulnerability scripts
```

**Metasploit:**
```bash
msfconsole                           # Start
search [keyword]                     # Find exploits
use [exploit]                        # Select exploit
show options                         # View settings
set [option] [value]                 # Set parameter
exploit                              # Run exploit
```

**SSH / Database:**
```bash
ssh user@host                        # Connect via SSH
mysql -h host -u user -p [pass]     # Connect to MySQL
psql -h host -U user                # Connect to PostgreSQL
```

---

### Learning Resources

**During the Project:**
- https://www.exploit-db.com (find exploits)
- https://nvd.nist.gov (vulnerability database)
- https://www.metasploit.com (tool docs)
- https://tryhackme.com (practice labs)

**For Deep Learning:**
- OWASP Top 10 (web vulnerabilities)
- NIST Cybersecurity Framework
- CIS Controls (what to defend against)
- CEH Exam Guide (if you want certification)

---

## FINAL CHECKLIST

**Before You Submit:**

**Technical Deliverables:**
- [ ] Reconnaissance report completed
- [ ] Vulnerability scan exported from Nessus
- [ ] At least 5 vulnerabilities successfully exploited
- [ ] Privilege escalation demonstrated
- [ ] Data exfiltration completed
- [ ] Screenshots for each phase
- [ ] Command logs documented

**Report Deliverables:**
- [ ] Executive Summary (2-3 pages)
- [ ] Technical Report (20+ pages with details)
- [ ] 8-Slide Presentation
- [ ] Evidence folder organized
- [ ] README explaining everything

**Presentation Quality:**
- [ ] No spelling/grammar errors
- [ ] Professional formatting
- [ ] Proper file naming
- [ ] All evidence included
- [ ] Clear explanations

**Personal Preparation:**
- [ ] You can explain what you did
- [ ] You understand each vulnerability
- [ ] You can defend your methodology
- [ ] You're ready for questions

---

**Remember:**

**Every step might feel hard in the moment, but each one teaches you something real that most people will never know.**

**You're not just learning cybersecurity. You're BECOMING a cybersecurity professional.**

**That's awesome.**

**Now go hack something (ethically)!** 🎯

-
**Thank you for being part of this project. You're going to do great work. I can feel it.** ✨

**Now go get started!**

---

*Questions? Stuck? Don't understand something? Reach out during Q&A sessions or office hours. There are no dumb questions in cybersecurity—only security professionals who ask them and learn.* 🛡️
