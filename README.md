# 🚨 Project 6 — Incident Response + MITRE ATT&CK Mapping

![Case ID](https://img.shields.io/badge/Case%20ID-IR--2026--006-blue?style=for-the-badge)
![Severity](https://img.shields.io/badge/Severity-HIGH-red?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-RESOLVED-brightgreen?style=for-the-badge)
![Framework](https://img.shields.io/badge/Framework-MITRE%20ATT%26CK%20v14-orange?style=for-the-badge)

---

## 📌 Project Overview

This project documents a **complete Incident Response investigation** following a simulated RDP brute force attack in a personal cybersecurity home lab. The investigation follows the **NIST SP 800-61 Incident Response Lifecycle** and maps all attacker techniques to the **MITRE ATT&CK v14 Framework**.

This project ties together evidence from two prior lab projects:
- 🔗 **Project 3** — [Brute Force Attack Detection](../brute-force-attack-detection/) `BFA-2026-003`
- 🔗 **Project 5** — [Splunk SIEM Monitoring](../splunk-siem-monitoring/) `SIEM-2026-004`

---

## 🎯 Objectives

- Conduct a structured Incident Response investigation using real lab evidence
- Apply the NIST SP 800-61 six-step IR lifecycle to a real attack scenario
- Map all observed attacker techniques to MITRE ATT&CK v14
- Document a professional IR report suitable for a SOC analyst portfolio

---

## 🖥️ Lab Environment

| Component | Details |
|---|---|
| Host OS | Windows 11 (Host Laptop) |
| Virtualisation | Oracle VirtualBox |
| Attacker Machine | Kali Linux 2026.1 — IP: `192.168.0.141` |
| Victim Machine | Windows 10 VM — IP: `192.168.0.138` — Hostname: `DESKTOP-NJB72C5` |
| SIEM Platform | Splunk Enterprise 10.2.2 |
| Attack Tool | Hydra v9.6 |
| Target Service | RDP (Remote Desktop Protocol) — Port `3389` |

---

## 📋 Incident Summary

```
SCENARIO:
A Windows 10 VM was targeted by an RDP brute force attack launched from
a Kali Linux attacker machine using Hydra v9.6.

The attacker systematically tried username and password combinations
against port 3389 until the testuser account was compromised.

Splunk SIEM detected the attack via 93 EventCode 4625 alerts.
Windows security triggered an account lockout (EventCode 4740).
```

---

## 🛡️ Incident Response — NIST SP 800-61 Lifecycle

### STEP 1 — PREPARATION ✅
- Splunk Enterprise configured to ingest Windows Security Event Logs
- Custom brute force alert rule created: `EventCode 4625 > 5 per hour`
- SOC Monitoring Dashboard built with 3 real-time panels
- Windows audit logging enabled

### STEP 2 — IDENTIFICATION ✅
- Splunk alert triggered: **93 EventCode 4625 events** detected
- **72 failed RDP login attempts** from `192.168.0.141` within 2 minutes
- Attacker workstation name `kali` visible in event logs
- EventCode 4740 captured — account lockout at `19-04-2026 12:32:20`

### STEP 3 — CONTAINMENT ✅
- Attacker IP `192.168.0.141` blocked at firewall level
- RDP service temporarily disabled
- `testuser` account locked immediately

### STEP 4 — ERADICATION ✅
- `testuser` password reset — `Password123` replaced
- System checked for persistence mechanisms and backdoors
- NTLM authentication logs reviewed for post-lockout anomalies

### STEP 5 — RECOVERY ✅
- RDP re-enabled with Network Level Authentication (NLA)
- RDP restricted to trusted IPs via firewall rules
- Account lockout policy enforced: max 5 attempts before lockout
- Systems monitored for 48 hours post-incident

### STEP 6 — LESSONS LEARNED ✅
- Weak passwords allow trivial brute force compromise
- Exposed RDP without MFA = critical attack surface
- Splunk SIEM successfully detected attack via automated alerting
- SOC Dashboard provided real-time attack visibility

---

## 🗺️ MITRE ATT&CK Mapping

| Technique ID | Technique Name | Tactic | Observed Evidence |
|---|---|---|---|
| [T1110](https://attack.mitre.org/techniques/T1110/) | Brute Force | Credential Access | 72 EventCode 4625 failures in 2 mins |
| [T1110.001](https://attack.mitre.org/techniques/T1110/001/) | Password Guessing | Credential Access | Hydra wordlist attack via passwords.txt |
| [T1021.001](https://attack.mitre.org/techniques/T1021/001/) | Remote Desktop Protocol | Lateral Movement | RDP port 3389 targeted from Kali Linux |
| [T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | Defence Evasion | `testuser:Password123` successfully cracked |
| [T1190](https://attack.mitre.org/techniques/T1190/) | Exploit Public-Facing Application | Initial Access | RDP exposed on network without MFA |

---

## 🔍 Key Findings

| Event ID | Event Name | Count | Significance |
|---|---|---|---|
| 4625 | Failed Logon | 72 | Brute force attempts from Kali IP |
| 4740 | Account Lockout | 1 | Windows security blocked the attack |
| 4624 | Successful Logon | 507 | Normal activity + post-attack access |

---

## 🚩 Indicators of Compromise (IOCs)

| IOC Type | Value | Description |
|---|---|---|
| Attacker IP | `192.168.0.141` | Kali Linux attacker machine |
| Target IP | `192.168.0.138` | Windows 10 VM victim machine |
| Target Port | `3389` | RDP — service under attack |
| Username | `testuser` | Account targeted by brute force |
| Password | `Password123` | Successfully cracked credential |
| Attack Tool | `Hydra v9.6` | Automated brute force tool |
| Event Pattern | 72 failures / 2 min | Classic brute force signature |

---

## 📁 Repository Structure

```
📁 incident-response-mitre-attack/
│
├── 📄 README.md                              
├── 📄 Incident-Response-MITRE-ATT&CK-Report.pdf  ← Full investigation report
│
└── 📁 evidence/
    ├── mitre-T1110.png        ← MITRE ATT&CK T1110 technique page
    ├── mitre-T1021-001.png              ← MITRE ATT&CK T1021.001 technique page
    ├── mitre-T1078.png     ← MITRE ATT&CK T1078 technique page
    ├── mitre-T1190.png     ← MITRE ATT&CK T1190 technique page
```

> 📌 **Additional evidence** (Hydra attack, Event Logs, Splunk searches) is available in:
> - [Project 3 — Brute Force evidence folder](../brute-force-attack-detection/evidence/)
> - [Project 5 — Splunk SIEM evidence folder](../splunk-siem-monitoring/evidence/)

---

## 🛠️ Tools & Resources Used

| Tool | Purpose |
|---|---|
| [MITRE ATT&CK](https://attack.mitre.org) | Technique mapping framework |
| Splunk Enterprise 10.2.2 | SIEM detection platform |
| Windows Event Viewer | Log analysis |
| Hydra v9.6 | Brute force tool (attacker simulation) |
| Oracle VirtualBox | Lab virtualisation |

---

## 💡 Key Takeaways

- ✅ Incident Response is a **structured process** — not guesswork
- ✅ MITRE ATT&CK maps **real hacker behaviour** to documented technique IDs
- ✅ Splunk SIEM is effective at **automatically detecting** brute force patterns
- ✅ Weak passwords + exposed RDP = **critical risk** in any environment
- ✅ Proper Windows audit logging provides a **clear evidence trail** for investigations

---

## ⚠️ Disclaimer

> This project was conducted in an **isolated virtual lab environment** for educational purposes only. All simulated attacks were performed on machines owned and controlled by the analyst. No real systems, networks or individuals were targeted or harmed.

---

*Prepared by: **Meenakshy B R** | Aspiring SOC Analyst | [github.com/Meenakshy10](https://github.com/Meenakshy10)*
