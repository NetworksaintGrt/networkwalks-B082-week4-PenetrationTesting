# Networkwalks-B082-week4-mediroza-pentest

Traced a single username-enumeration bug through SQL injection, PDF cracking, and metadata leakage to full exposure of a hospital's patient records, staff payroll, and shareholder data. Full black-box engagement against an authorized capstone target, documented end-to-end with proof of exploitation and remediation.

Week 4 — Web Application Penetration Testing

Networkwalks Academy — Cybersecurity & Ethical Hacking Internship

---

Status: Completed
Internship: Cybersecurity & Ethical Hacking
Tool Category: Web App Pentesting & Vulnerability Chaining
Modules: 7 Findings
License: Educational Use

---

📖 Overview

This repository documents my Week 4 capstone project: a black-box penetration test of Mediroza General Hospital's public-facing patient portal (`medirozahospital.com`), performed with the client's written authorization. The engagement followed a standard four-phase methodology — Reconnaissance, Vulnerability Identification, Exploitation, and Documentation — and surfaced seven issues ranging from Medium to Critical severity.

| Phase | Focus | Tools |
|---|---|---|
| Recon | Public information gathering, hidden paths via robots.txt | Browser, curl |
| Vulnerability ID | Login behavior, input handling | Browser DevTools, curl |
| Exploitation | Auth bypass, PDF cracking, metadata & backup extraction | Networkwalks Hash Calculator/Cracker, qpdf, exiftool, wget |
| Documentation | Evidence capture, attack chain, remediation | Claude (data formatting) |

Note: This was a supervised training exercise. The target was authorized in advance for testing; none of these techniques may be used against any system without the owner's explicit written permission.

---

🧠 Background

This engagement shows how small, individually low-impact issues chain into a critical breach. It started with a login page leaking whether a username existed, moved through a classic SQL injection auth bypass, and ended with a publicly browsable backup folder exposing a decade-old database dump — full of unencrypted staff salaries and shareholder ownership stakes.

The chain in short: **enumerate → inject → download → crack → pivot via metadata → find the exposed backup → read everything.**

---

📂 Repository Structure

```
week4-mediroza-pentest/
├── README.md
├── report.pdf                          # Full write-up: methodology, evidence, remediation
├── findings/
│   ├── 01-username-enumeration/
│   ├── 02-sql-injection-login-bypass/
│   ├── 03-pdf-reports-exposed/
│   ├── 04-weak-pdf-passwords/
│   ├── 05-pdf-metadata-leak/
│   ├── 06-directory-listing-backup/
│   └── 07-db-backup-plaintext-data/
└── screenshots/                        # Numbered evidence per finding
```

---

🏆 Results

| # | Finding | Location | Risk |
|---|---|---|---|
| 1 | Username enumeration on login page | `patient/login.php` | Medium |
| 2 | SQL injection — full login bypass | `patient/login.php` | **Critical** |
| 3 | Confidential PDFs reachable post-bypass | `patient/reports/` | High |
| 4 | Weak, wordlist-crackable PDF passwords | `patient_report_*.pdf` | High |
| 5 | Sensitive metadata left in a patient PDF | `patient_report_3.pdf` | Medium |
| 6 | Backup folder with directory listing enabled | `/old` | **Critical** |
| 7 | Staff pay & shareholder data stored in plain text | `mediroza_db_backup_2019.sql` | **Critical** |

**Overall Risk Rating: 🔴 Critical**

---

💡 Key Takeaways

· A single unhandled apostrophe in a login field was enough to bypass authentication entirely — input validation is not optional
· Encryption only helps if the password behind it is strong; weak PDF passwords fell to a default wordlist in seconds
· Metadata is a real leak vector — an internal IT comment embedded in a PDF handed over the location of a sensitive backup
· Directory listings and forgotten backups inside the web root turn a contained bypass into a full data breach
· Small findings compound — no single issue here was catastrophic alone, but chained together they reached patient records, payroll, and ownership data
· Parameterized queries, generic error messages, and keeping backups out of the web root would have stopped this chain at step one

---

🙏 Acknowledgements

Special thanks to Networkwalks Academy and my instructor Waqas Karim (CCIE) for their guidance and mentorship throughout this engagement.

---

👤 Author

NetworkSaint@Grt (Like Muyambango)
Cybersecurity & Ethical Hacking Intern — Networkwalks Academy

---

📌 Project Information

| Attribute | Details |
|---|---|
| Organization | Networkwalks Academy |
| Program | Cybersecurity & Ethical Hacking Internship |
| Week | 04 |
| Project Type | Web Application Penetration Test — Capstone |

---

This repository is for educational purposes only. This assessment was conducted with the target owner's explicit written authorization as part of a supervised training program. These techniques must never be applied to any system without explicit written permission from its owner.
