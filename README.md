# bnmap 🔍

> **bnmap** — Smart nmap wrapper for cybersecurity students

```
  _____         _  _  _  _  ___  ____  
 |  __ \       | \| || \| ||   \|  _ \ 
 | |__) |      |  ` ||  ` || () | |_) |
 |_____/ nmap  |_|\_||_|\_||___/|____/ 
  Smart nmap wrapper — for cybersecurity students
```

**bnmap** is an intelligent nmap wrapper that analyzes your scan command, recommends missing flags interactively, runs the scan, and automatically gives you targeted next-step attack recommendations based on what ports are open — all in one command.

---

## Features

- **Command analyzer** — detects missing flags (-p-, -sV, -sC, -O, -sU, -T4, -oA) and scores your scan coverage
- **Interactive flag menu** — choose which flags to add, pick specific ones, type custom flags, or run as-is
- **Smart recommendations** — for every open port, gives 5 specific attack commands to run next
- **Auto report** — saves every scan to a timestamped `.txt` file automatically
- **No API needed** — works 100% offline with local port-based intelligence

---

## Supported Ports & Attack Suggestions

| Port | Service | What bnmap suggests |
|------|---------|-------------------|
| 21 | FTP | Anonymous login, hydra brute, vsftpd backdoor, wget download, NSE scripts |
| 22 | SSH | searchsploit, hydra brute, default creds, auth methods check, key login |
| 23 | Telnet | Default creds, hydra brute, tcpdump sniff, NSE scripts, metasploit |
| 25/587 | SMTP | VRFY enum, smtp-user-enum, open relay test, NSE scripts, metasploit |
| 53 | DNS | Zone transfer, reverse lookup, subdomain brute, dnsrecon, NSE scripts |
| 80/8080 | HTTP | gobuster, feroxbuster, nikto, ffuf param fuzz, NSE http scripts |
| 443/8443 | HTTPS | sslscan, cert extraction, gobuster, ssl vuln NSE scripts, nikto |
| 139/445 | SMB | enum4linux, smbmap, EternalBlue check, smbclient, crackmapexec |
| 389/636 | LDAP | Anonymous enum, naming contexts, user enum, NSE scripts, bloodhound |
| 1433 | MSSQL | mssqlclient, hydra, NSE scripts, xp_cmdshell RCE |
| 3306 | MySQL | root login, hydra, file read, webshell write |
| 3389 | RDP | BlueKeep check, hydra, xfreerdp, crackmapexec, encryption check |
| 5985/5986 | WinRM | evil-winrm, pass-the-hash, crackmapexec, metasploit |
| 6379 | Redis | Unauthenticated access, SSH key write, crontab reverse shell |
| 27017 | MongoDB | Unauthenticated access, dump databases, NSE scripts |
| Any | Unknown | searchsploit, banner grab, NSE vulners, exploit-db link |

---

## Installation

```bash
# Clone the repo
git clone https://github.com/manmind34/bnmap.git
cd bnmap

# Install
sudo cp bnmap /usr/local/bin/bnmap
sudo chmod +x /usr/local/bin/bnmap
```

---

## Usage

```bash
bnmap <target> [extra flags]
```

### Examples

```bash
bnmap 192.168.1.1
bnmap 10.10.10.10 -p 80,443,445
bnmap 192.168.1.0/24
bnmap target.com -sV -p 22,80
```

---

## How it works

When you run `bnmap`, it will:

1. **Analyze** your command — check for missing flags and score your scan coverage
2. **Show a menu** — let you choose which flags to add:
   ```
   [1] -p-    Full port scan
   [2] -sV    Version detection
   [3] -sC    Default NSE scripts
   ...
   [a] Add all    [c] Custom flags    [n] Run as-is
   ```
3. **Run the scan** — executes nmap with your chosen flags live in terminal
4. **Recommend next steps** — for every open port found, prints 5 specific attack commands
5. **Save report** — auto-saves full scan output to `bnmap_TARGET_TIMESTAMP.txt`

---

## Example Output

```
[bnmap] Analyzing your command...

  [!] Missing -p-    Only default 1000 ports scanned
  [!] Missing -sV    Without it you only see port numbers
  [*] Consider -sC   NSE scripts: banner grabs, vuln checks
  ...

  Scan coverage: ████░░░░░░░░░░░░░░░░ 20%

  [a] Add all missing flags (recommended)
  [1-7] Add specific flags
  [c] Type your own custom flags
  [n] Run as-is

[bnmap] Running scan...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PORT    STATE  SERVICE  VERSION
22/tcp  open   ssh      OpenSSH 7.4
80/tcp  open   http     Apache 2.4.29
445/tcp open   smb      Samba 4.7
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[bnmap] Recommended next steps:

  [ SSH — port 22 ]
  [>] 1. Check version for known CVEs:
         searchsploit openssh
  [>] 2. Brute force with Hydra:
         hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://TARGET

  [ HTTP — port 80 ]
  [>] 1. Directory brute force:
         gobuster dir -u http://TARGET -w /usr/share/wordlists/dirbuster/...
  ...
```

---

## Requirements

- `nmap` — `sudo apt install nmap`
- `curl` — `sudo apt install curl`
- `jq` (optional) — `sudo apt install jq`

Recommended tools for follow-up attacks (not required):
`hydra` `gobuster` `feroxbuster` `nikto` `enum4linux` `smbclient` `smbmap` `crackmapexec` `evil-winrm` `searchsploit`

---

## Tested On

- Kali Linux 2024+
- Parrot OS
- Ubuntu 22.04+

---

## Author

Made by **manmind34** — cybersecurity student, CTF player

---

## License

MIT License — use it, share it, improve it.
