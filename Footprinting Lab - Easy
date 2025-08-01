
# 🛡️ Target 1 – Internal DNS/FTP Server – Inlanefreight Ltd (Footprinting Lab - Easy)

## 📝 Summary

During the initial assessment of Inlanefreight Ltd’s internal infrastructure, an internal host (`10.129.236.4`) was tested under **non-aggressive, enumeration-only** rules of engagement. The goal was to gather intelligence from exposed services and use this passively acquired information to gain internal access, without using direct exploitation.

A successful login via FTP using provided credentials exposed sensitive SSH keys, which led to valid SSH access and eventual discovery of the target `flag.txt`.

---

## 📋 Scope

- **Target IP**: `10.x.202.x`
- **Client Constraints**: No exploitation allowed (passive/credential-based access only)
- **Objective**: Gather as much intelligence as possible and retrieve `flag.txt` as proof of access.

---

## ⚙️ Enumeration

### 🔍 Nmap Scan

Command:
```bash
sudo nmap -sC -sV 10.x.202.x
```

**Open Ports**:
| Port | Service | Version |
|------|---------|---------|
| 21 | FTP | ProFTPD (ftp.int.inlanefreight.htb) |
| 22 | SSH | OpenSSH 8.2p1 (Ubuntu) |
| 53 | DNS | ISC BIND 9.16.1 (Ubuntu) |
| 2121 | FTP | ProFTPD (Ceil’s FTP) |

Key Observations:
- Port `2121` banner explicitly references **Ceil’s FTP**, suggesting a user-specific service.
- Port `22` open with standard SSH.
- Provided credentials (`ceil:q****4`) suggest testing user-level access.

---

## 🔑 Authentication & Access

### 1️⃣ FTP Access via Port 2121

Command:
```bash
ftp 10.x.202.x 2121
```
Credentials:
```text
Username: ceil
Password: q***4
```

- Login was successful.
- Initial `ls` showed no files.
- Using `ls -la`, revealed:
  - `.bash_history`
  - `.ssh/` directory
  - `id_rsa` (user's private SSH key)

Downloaded `id_rsa`:
```bash
get .ssh/id_rsa
chmod 600 id_rsa
```

### 2️⃣ SSH Access via Private Key

Used downloaded private key for SSH authentication:
```bash
ssh -i id_rsa ceil@10.x.202.x
```

- Login as `ceil` was successful.
- Navigated upward from user home directory.
- Located `flag.txt` in `/flag/` directory.

---

## 🧾 Proof of Access

```bash
cat /flag/flag.txt
HTB{example-redacted-flag}
```

---

## 🛠️ Analysis & Risk

### Finding: **Exposed Private Key on FTP Server**

| Type | Misconfiguration |
|------|------------------|
| Severity | High |
| Affected Port | 2121 (ProFTPD - Ceil’s FTP) |
| Description | User's private SSH key was exposed in FTP directory |
| Impact | Unauthorized remote access via SSH with full user privileges |
| Recommendation | Remove sensitive files from FTP directories, rotate SSH keys, and isolate credentials between services |

---

## ✅ Conclusion

The internal server at `10.x.202.x` allowed passive enumeration and credential-based access without requiring any active exploitation. The reuse of weak credentials (`ceil:q****4`) combined with improper SSH key storage led to full user access and successful mission completion with retrieval of `flag.txt`.
