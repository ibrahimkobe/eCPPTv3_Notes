# 🛡️ eCPPT Study Notes

Personal study notes compiled while preparing for the **eLearnSecurity Certified Professional Penetration Tester (eCPPT)** certification. These notes go beyond the eJPT fundamentals into deeper network pentesting, Windows/Linux privilege escalation, lateral movement & pivoting, PowerShell for offensive use, Active Directory attacks, and web application exploitation.

> ⚠️ **Disclaimer:** These are personal study notes for educational purposes only. All techniques referenced are intended for use in **authorized lab environments** (e.g. INE/eLearnSecurity labs, Hack The Box, personal test labs) only. Do not use any of this information against systems you do not own or do not have explicit written permission to test. Unauthorized access to computer systems is illegal.

---

## 📑 Table of Contents

- [About This Repo](#-about-this-repo)
- [Contents](#-contents)
- [Topics Covered](#-topics-covered)
- [Penetration Testing Methodology](#-penetration-testing-methodology)
- [Tools Referenced](#-tools-referenced)
- [How I Use These Notes](#-how-i-use-these-notes)
- [Exam Prep Tips](#-exam-prep-tips)
- [Resources](#-resources)
- [Feedback / Contributing](#-feedback--contributing)

---

## 📘 About This Repo

The **eCPPT (eLearnSecurity Certified Professional Penetration Tester)** is an intermediate-to-advanced, hands-on penetration testing certification that builds on eJPT fundamentals with a strong focus on **Active Directory attacks, Windows/Linux privilege escalation, lateral movement, pivoting, and PowerShell-based offense**. This repository is my personal knowledge base built while working through the course modules, HackTheBox-style labs, and practical exercises.

Each file includes:
- Step-by-step command sequences taken from real lab walkthroughs
- Tool syntax, flags, and parameter explanations
- "Gotchas" and troubleshooting notes
- Key terminology and attack-technique explanations

---

## 📖 Contents

| # | Topic | File | Covers |
|---|-------|------|--------|
| 1 | PowerShell for Pentesters | [1. PowerShell For Pentesters](1__Powershell_For_Pentesters.md) | PowerShell fundamentals, cmdlets, modules, scripting for offensive use |
| 2 | Network Penetration Testing | [Network Penetration Testing](Network_Penetration_Testing.md) | DNS recon, host/port scanning, service enumeration, NSE, IDS evasion, SNMP, MSSQL attacks |
| 3 | Privilege Escalation | [1. Privilege Escalation](1__Privilege_Escalation.md) | Windows & Linux privesc techniques, credential hunting, post-exploitation |
| 4 | Active Directory Penetration Testing | [Active Directory Penetration Testing](Active_Directory_Penetration_Testing.md) | AD enumeration, Kerberoasting, lateral movement, trust attacks |
| 5 | Lateral Movement & Pivoting | [2. Lateral Movement & Pivoting](2__Lateral_Movement___Pivoting.md) | PsExec, SMBExec, CrackMapExec, RDP, WinRM, Pass-the-Hash, pivoting, tunneling |
| 6 | Web Application Penetration Testing | [Web Application Penetration Testing](Web_Application_Penetration_testing.md) | WHOIS/DNS enumeration, fingerprinting, XSS, SQLi, OWASP ZAP/SQLMap, Amass |

---

## 🧭 Topics Covered

### 💻 PowerShell for Pentesters
- What PowerShell is, scripts vs. cmdlets vs. CLI, version considerations
- 32-bit vs. 64-bit executables, running as administrator
- CLI usage: command-line parameters, parameter abbreviation
- `Get-Help`, `Get-Command`, cmdlet structure and default output
- Pipelining and redirecting output to a file
- Useful cmdlets: `Get-Process`, `Get-ChildItem`, `Get-WmiObject`, `Export-Csv`, registry exploration, `Select-String`, `Get-Content`, `Get-Service`
- Modules: components, types, `Get-Module`, `Import-Module` (case study: importing PowerSploit)
- Writing and running basic scripts, including argument handling

### 🌐 Network Penetration Testing
- Active information gathering: DNS zone transfers & interrogation
- Host discovery (most effective ways to confirm a host is up)
- Nmap: scan types, TCP commands, ping sweeps, SMB-specific commands
- Service version & OS detection
- Nmap Scripting Engine (NSE) usage
- Firewall detection & **IDS evasion** techniques
- Optimizing Nmap scans for speed/stealth, output formats
- Port scanning & enumeration integrated with Metasploit (`db_import`, auxiliary modules)
- Service enumeration: FTP, SMB, Web, MySQL, SSH, SMTP
- SNMP enumeration
- DNS & SMB relay attacks
- **MSSQL DB user impersonation → RCE**

### 🔓 Privilege Escalation
**Windows:**
- `PowerUp` and `PrivescCheck` automated enumeration
- Unattended installation file hunting (`unattend.xml`)
- Windows Credential Manager abuse
- PowerShell history mining
- Exploiting insecure service permissions
- Registry AutoRun privilege escalation
- Access Token Impersonation
- Juicy Potato (token impersonation → SYSTEM)
- Locally stored credentials

**Linux:**
- Misconfigured file permissions
- Exploiting SUID binaries
- Misconfigured `sudo` privileges

**Post-Exploitation (both platforms):**
1. Persistence
2. Hash cracking
3. Credential dumping
4. Internal enumeration
5. Further privilege escalation

### 🏢 Active Directory Penetration Testing
**eCPPTv3-style AD attack chain:**
1. **Initial Access** — Password spraying
2. **Enumeration** — BloodHound/SharpHound, PowerView
3. **Privilege Escalation** — AS-REP Roasting, Kerberoasting
4. **Lateral Movement** — Pass-the-Hash (PtH), Pass-the-Ticket (PtT)

**Extended AD attacks (HTB-style):**
- Username enumeration with Kerbrute
- LLMNR/NBT-NS poisoning
- Password spraying & password policy enumeration
- Enumerating security controls
- Credentialed enumeration & Living Off the Land techniques
- File transfer techniques
- Kerberoasting & ASREPRoasting
- ACL enumeration & abuse tactics
- DCSync attack
- Privileged access techniques
- **NoPac**, **PrintNightmare**, **PetitPotam**
- Miscellaneous AD misconfigurations
- Group Policy enumeration & attacks
- Trust relationship attacks: child→parent and cross-forest trusts

### 🔀 Lateral Movement & Pivoting
- **PsExec** — SMB brute-force + authenticated remote execution, `web_delivery` for meterpreter shells
- **SMBExec** — password & pass-the-hash authentication, `hta_server` delivery
- **CrackMapExec** — command execution (CMD vs PowerShell), SAM dumping, `web_delivery` module integration
- **RDP lateral movement** — exploiting BadBlue, `SharpDPAPI` for credential decryption, `kiwi`/Mimikatz DPAPI master-key extraction, `xfreerdp` sessions
- **WinRM** — `evil-winrm`, loading Invoke-Mimikatz, PS Remoting / `Enter-PSSession`
- **Pass-the-Hash** with Metasploit `psexec` and `wmiexec.py`
- **Linux lateral movement** — SSH key reuse, `scp`, Firefox credential decryption (`firepwd.py`), SQLite credential extraction, privilege chains across multiple hosts
- **Pivoting & port forwarding** with Metasploit (`autoroute`, `portfwd`)
- **SOCKS proxy pivoting** (`proxychains`, `socks4a`)
- **SSH tunneling** for pivoting (dynamic port forwarding `-D`)

### 🌐 Web Application Penetration Testing
- WHOIS & passive DNS enumeration, DNS record types, DNS enumeration tools
- Reviewing webserver metafiles
- Web app & web server technology fingerprinting
- Web server vulnerability scanning
- File & directory brute-forcing
- Automated recon with **OWASP Amass**
- **XSS exploitation:**
  - Reflected XSS
  - Stored XSS
  - DOM-based XSS (sources, sinks, concrete examples, prevention)
- **SQL Injection:**
  - Finding SQLi with OWASP ZAP
  - Exploiting with **SQLMap** (workflow summary)
  - Quick comparison of manual vs. automated approaches

---

## 🔄 Penetration Testing Methodology
Network Recon & Scanning → DNS, host discovery, port/service scanning, NSE, evasion
Service Enumeration → FTP, SMB, HTTP, MySQL, SSH, SMTP, SNMP, MSSQL
Web App Testing → Fingerprinting, brute force, XSS, SQLi
Exploitation → Gain initial foothold
Privilege Escalation → Windows/Linux local privesc techniques
Active Directory Attacks → Enumeration, Kerberoasting, ACL abuse, trust attacks
Lateral Movement & Pivoting → PsExec, WinRM, RDP, SOCKS/SSH pivoting
Post-Exploitation & Persistence → Credential dumping, hash cracking, persistence

**Typical AD Attack Chain (from notes):**
Password Spraying → BloodHound/PowerView Enumeration → Kerberoasting/AS-REP Roasting → Pass-the-Hash / Pass-the-Ticket → Domain Dominance (DCSync)

**Typical Pivoting Chain (from notes):**
Initial foothold (Metasploit exploit) → `autoroute` → SOCKS proxy / `portfwd` → `proxychains` to reach internal hosts → repeat on next segment

---

## 🛠️ Tools Referenced

| Category | Tools |
|---|---|
| Scripting / Offense | `PowerShell`, `PowerSploit`, `PowerUp`, `PrivescCheck` |
| Network Scanning | `Nmap` (NSE, IDS evasion, service/OS detection) |
| Exploitation Framework | `Metasploit Framework (msfconsole)` |
| Credential Attacks | `Hydra`, `crackmapexec`, `kiwi`/`Mimikatz`, `SharpDPAPI` |
| Lateral Movement | `psexec.py`, `smbexec.py`, `wmiexec.py`, `evil-winrm`, `xfreerdp` |
| AD Enumeration | `BloodHound`, `SharpHound`, `PowerView`, `Kerbrute` |
| Pivoting | `proxychains`, Metasploit `autoroute`/`portfwd`, SOCKS4a, SSH dynamic tunneling |
| Web Recon | `OWASP Amass`, WHOIS, DNS enumeration tools |
| Web App Testing | `OWASP ZAP`, `SQLMap`, manual XSS/SQLi testing |
| Linux Post-Exploitation | SSH key reuse, `scp`, `firepwd.py`, `sqlite3` |

---

## 📝 How I Use These Notes

- **Before labs:** Review the relevant module for command syntax, tool flags, and attack chain order.
- **During labs:** Keep the file open as a live reference for exact Metasploit module paths, PowerShell cmdlets, and lateral movement commands.
- **After labs:** Append new "gotchas" (e.g. wrong hash format, WinRM vs PS Remoting confusion) back into the notes.
- **Before the exam:** Walk through the full attack chain end-to-end — recon → foothold → privesc → AD attacks → lateral movement → domain dominance.

---

## 🎯 Exam Prep Tips

- For `psexec`/`smbexec`, remember the account **must be a local/domain Administrator** — a common early blocker in labs.
- When brute-forcing is slow or fails, pivot to enumerating valid usernames first (Kerbrute, SMB null sessions) to narrow the attack.
- Pass-the-Hash requires the **NT hash paired with an all-zero LM hash** (`00000000000000000000000000000000:NTHASH`) — a frequent formatting mistake.
- `evil-winrm` connecting successfully doesn't mean `Enter-PSSession` will work — **PS Remoting must be explicitly enabled** (`Enable-PSRemoting`) on the target even if WinRM is up.
- For pivoting, always confirm reachability with `ping` *before and after* setting up `autoroute` — it's the fastest way to confirm your route is actually working.
- In AD environments, always run **BloodHound** early — it often reveals attack paths (ACL abuse, unconstrained delegation) faster than manual enumeration.
- Chain enumeration constantly on Linux lateral movement labs: SSH keys, `.bash_history`, browser credential stores (Firefox `key4.db` + `logins.json`), and SQLite DBs are common pivot points between users.

---

## 📚 Resources

- [eLearnSecurity eCPPT Official Page](https://elearnsecurity.com/product/ecppt-certification/)
- [Metasploit Unleashed (Offensive Security)](https://www.offensive-security.com/metasploit-unleashed/)
- [HackTricks](https://book.hacktricks.xyz/)
- [GTFOBins](https://gtfobins.github.io/)
- [BloodHound Documentation](https://bloodhound.readthedocs.io/)
- [PowerSploit (GitHub)](https://github.com/PowerShellMafia/PowerSploit)
- [Active Directory Pentesting Notes — Hack The Box style paths](https://www.hackthebox.com/)

---

## ⭐ Feedback / Contributing

These are personal study notes, but if you spot an error, outdated command, or want to suggest an addition, feel free to open an issue or submit a pull request.

---

🧑‍💻 *Maintained as part of my eCPPT certification prep journey.*
