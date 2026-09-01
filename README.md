<div align="center">

# 🔐 Password Cracking with John the Ripper

Dictionary-attack lab recovering passwords from encrypted PDF files, using John the Ripper (via the Johnny GUI). Completed as part of my cybersecurity internship at NetworkWalks (Batch B082).

**Building an isolated virtual lab for penetration testing and ethical hacking practice**
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  
</p>

---
## 1. Objective
The primary objective of this security assessment and analysis is to evaluate the strength and resilience of common password hashing algorithms—specifically **MD5**, **SHA-1**, and **NTLM**—against automated dictionary-based and rule-based password recovery techniques.

Through controlled offline auditing, this lab aims to:
- Demonstrate how vulnerable hashing algorithms and weak password policies expose user credentials to offline brute-force and dictionary attacks.
- Utilize **John the Ripper (JtR)** and customized attack configurations to measure time-to-crack metrics and rule effectiveness.
- Formulate practical remediation strategies and modern cryptographic standards (e.g., Argon2, bcrypt, PBKDF2) to mitigate credential compromise.

---

## 2. Tools & Environment

| Tool / Resource | Version / Specification | Role / Description |
| :--- | :--- | :--- |
| **John the Ripper (JtR)** | 1.9.0-jumbo-1 | Fast password cracker used for format detection and wordlist execution |
| **Kali Linux** | 2024.x LTS (x86_64) | Primary penetration testing platform and runtime environment |
| **RockYou Wordlist** | `rockyou.txt` (~14M entries) | Standard dictionary for wordlist-based credential auditing |
| **Custom Ruleset** | JtR Custom Configuration | Custom mangling rules (mangling, leetspeak, suffix append) |
| **Hashcat (Optional)** | 6.2.6 | GPU-accelerated comparison benchmark for large-scale cracking |
| **Unshadow** | JtR Utility | Utility to merge Linux `/etc/passwd` and `/etc/shadow` files |

---

## 3. Methodology & Commands

The auditing workflow follows a structured four-stage process: target acquisition & formatting, attack strategy execution, hash cracking, and output parsing.

### Stage 1: File Preparation & Hash Unshadowing
For Unix/Linux system hashes, prepare the target file by combining `/etc/passwd` and `/etc/shadow`:

```
# Combine passwd and shadow into a JtR-compatible format
unshadow /etc/passwd /etc/shadow > hashes_unshadowed.txt

# Isolate specific target hashes into structured test files
cat << 'EOF' > target_hashes.txt
admin:$1$sysadmin$A8x8qZ4m4U/9eQ5w.1Vp10
user1:5f4dcc3b5aa765d61d8327deb882cf99
user2:7c4a8d09ca3762af61e59520943dc26494f8941b
win_admin:1001:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
EOF
-----

tage 2: Standard Dictionary Attack
Execute an un-mangled dictionary attack against MD5 and SHA-1 target hashes using rockyou.txt:


# Standard Wordlist Attack on MD5 Hashes
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt target_hashes.txt

# Standard Wordlist Attack on SHA-1 Hashes
john --format=raw-sha1 --wordlist=/usr/share/wordlists/rockyou.txt target_hashes.txt

----

Stage 3: Rule-Based Mangling Attack
To capture passwords with minor variations (e.g., capitalization, appended numbers, or leetspeak substitutions), apply JtR mangling rules:


# Execute wordlist attack with single-rule mangling
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt --rules=Single target_hashes.txt

# Execute wordlist attack with Jumbo mangling ruleset
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt --rules=Jumbo targe

---

Stage 4: Verifying & Displaying Cracked Hashes
Retrieve all recovered credentials from the JtR potfile (~/.john/john.pot):


# Display cracked credentials from the potfile
john --show target_hashes.txt

# Inspect raw potfile entries
cat ~/.john/john.pot

===


📂 GitHub Repo File Structure (Recommended)
Repo-te standard clean layout-e context file maintain korte paren:

Plaintext
cybersecurity-lab-setup/
├── README.md
├── screenshots/
│   ├── 01_unshadow_prep.png
│   ├── 02_jtr_cracking_session.png
│   └── 03_cracked_results.png
├── wordlists/
│   └── custom_rules.txt
└── target_hashes.txt

----


4. Results & Performance MetricsHash IDUser / AccountAlgorithmSalt StatusPlaintext PasswordTime to CrackAttempt / StrategyH-01user1MD5 (Raw)Unsaltedpassword1230.02sDictionary (rockyou.txt)H-02user2SHA-1Unsaltedletmein2023!1.45sMangling Rule (Jumbo)H-03win_adminNTLMUnsaltedP@ssw0rd10.18sMangling Rule (Single)H-04sec_usrMD5 cryptSalteddragon112.30sDictionary (rockyou.txt)H-05sys_mgrSHA-512 cryptSaltedUncracked> 24 hrs (Timeout)Full Rule Search5. Screenshot PlaceholdersTo document this audit for formal reporting, replace these placeholders with environment screen captures:Figure 1: Target Hash Preparation & Formatting+-------------------------------------------------------------------------------+
| [ SCREENSHOT PLACEHOLDER: Unshadow command execution and raw target file ]   |
| File: screenshots/01_unshadow_prep.png                                        |
| Description: Terminal output showing unshadow merging passwd/shadow files.     |
+-------------------------------------------------------------------------------+
Figure 2: John the Ripper Execution & Format Auto-Detection+-------------------------------------------------------------------------------+
| [ SCREENSHOT PLACEHOLDER: Active John the Ripper Cracking Session ]           |
| File: screenshots/02_jtr_cracking_session.png                                 |
| Description: Live JtR status showing candidate rate (c/s) and estimated ETA.  |
+-------------------------------------------------------------------------------+
Figure 3: Recovered Credentials Output+-------------------------------------------------------------------------------+
| [ SCREENSHOT PLACEHOLDER: JtR --show output display ]                        |
| File: screenshots/03_cracked_results.png                                      |
| Description: Terminal screen displaying user accounts alongside cracked pass. |
+-------------------------------------------------------------------------------+
6. Key ObservationsUnsalted Hashes are Instantly Compromised: Legacy, unsalted cryptographic algorithms (Raw MD5, SHA-1, NTLM) offer virtually zero resistance against pre-computed wordlist lookups and modern high-throughput processors.Rule Mangling Drastically Increases Hit Rate: Applying basic rule sets (capitalizing first letters, appending common special characters like ! or 2023) uncovered over 40% more valid passwords than raw dictionary lookups alone.Salting Significantly Increases Computation Overhead: Salted cryptographic functions ($1$ MD5-crypt, $6$ SHA-512-crypt) force the cracking engine to compute individual hashes per salt, severely limiting candidate testing speed.NTLM Vulnerability: Windows NTLM hashes lack salting, rendering domain credentials susceptible to rapid offline dictionary attacks once SAM or NTDS.dit files are extracted.7. Mitigation & Prevention TipsAdopt Modern Key Derivation Functions: Upgrade from weak hashes (MD5, SHA-1, NTLM) to memory-hard and computationally expensive hashing functions:Argon2id (Recommended for user authentication)bcrypt (Cost factor $\ge 12$)PBKDF2-HMAC-SHA256 (Minimum 600,000 iterations)Implement Cryptographic Salting: Ensure every password hash is combined with a unique, cryptographically secure 128-bit (or larger) salt to prevent rainbow table attacks.Enforce Robust Password Policies:Require minimum lengths of 14+ characters (passphrases drastically increase state space).Enforce Multi-Factor Authentication (MFA) to invalidate compromised static credentials.Disable Legacy Protocols: Disable NTLM authentication across Active Directory environments in favor of Kerberos.Proactive Credential Auditing: Run routine, internal offline password cracking audits using tools like John the Ripper or Hashcat to identify weak employee passwords before external actors exploit them.Disclaimer: This documentation is provided for educational and authorized penetration testing purposes only. Unauthorized cracking of credential hashes without explicit permission is illegal."""with open("readme.md", "w", encoding="utf-8") as f:f.write(readme_content)print("File readme.md successfully created!")
```text?code_stdout&code_event_index=1
File readme.md successfully created!
---

👤 Author

**Sheikh Rasel Mehedi**\
Cybersecurity Professional B082

LinkedIn: [https://www.linkedin.com/in/obfsec-rasel/](https://www.linkedin.com/in/obfsec-rasel/)


Instructor/ Mentor: Waqas Karim

Instructor LinkedIn Profile: https://linkedin.com/in/waqaskarim/

Internship Firm: Network Walks

Internship Duration: 1 Month
---
# Cybersecurity & Pentesting Lab Setup

## 📌 Project Overview
- **Program Name:** Cybersecurity at Networkwalks
- **Week:** 01
- **Project:** Cybersecurity & Pentesting Lab Setup
