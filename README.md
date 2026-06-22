```markdown
# ─── ❖ HACK THE BOX & OFFENSIVE SECURITY VAULT ❖ ───

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Hack%20The%20Box-9fe2bf?style=for-the-badge&logo=hack-the-box&logoColor=white" alt="HTB">
  <img src="https://img.shields.io/badge/Environment-Kali%20Linux-blueviolet?style=for-the-badge&logo=kali-linux&logoColor=white" alt="Kali">
  <img src="https://img.shields.io/badge/Tool-Obsidian-purple?style=for-the-badge&logo=obsidian&logoColor=white" alt="Obsidian">
</p>

This repository serves as my centralized vault for documentation, methodologies, and write-ups of compromised machines across **Hack The Box** and other offensive security training platforms.

The entire structure is natively managed using **Obsidian**, acting as a dynamic knowledge base for reconnaissance, exploitation, and post-exploitation workflows.

---

## 📂 Vault Structure

The laboratory organization follows a logical hierarchy optimized for tactical efficiency during an intrusion:

```text
.
├── 📁 01-Methodologies/      # Step-by-step guides (PrivEsc, Pivoting, AD)
├── 📁 02-Cheat-Sheets/       # Quick commands (Nmap, Reverse Shells, One-Liners)
├── 📁 03-Platforms/
│   ├── 📁 HackTheBox/
│   │   ├── 📁 Linux/         # Write-ups organized by OS and difficulty
│   │   └── 📁 Windows/
│   └── 📁 TryHackMe/
└── 📁 04-Tools/              # Specific notes and usage for hacking tools

```

---

## 🛠️ Integrated Post-Exploitation Workflow

My privilege escalation and lateral movement notes leverage industry-standard reference methodologies directly:

* **Internal Enumeration:** Manual checklists and automated enumeration scripts.
* **PrivEsc Vectors:** Exploitation of misconfigurations (SUID, capabilities, Cron jobs, token impersonation).
* **Pivoting Strategies:** Dynamic tunneling infrastructure using `Chisel`, `Proxychains`, and `Ligolo-ng`.

---

## ⚙️ Automated Sinking

Synchronization between environments is handled transparently via the **Obsidian Git** community plugin, performing automated background commits utilizing SSH cryptographic keys to maintain real-time laboratory integrity.

---
