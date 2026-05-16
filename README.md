# SMTP User Enumeration – HTB / Lab Writeup

## 📌 Overview

This project documents the enumeration of an SMTP service and the identification of a valid username using multiple enumeration techniques.

The assessment focused on:
- SMTP service analysis
- RCPT enumeration
- VRFY enumeration
- Catch-all detection
- Timeout tuning
- Manual SMTP interaction

---

# 🎯 Objective

Enumerate the SMTP service further and identify a valid username on the target system.

---

# 🔍 Service Discovery

## Nmap Scan

```bash
nmap -p25 -sV -sC <TARGET-IP>
```

### Findings

- SMTP service running on port 25
- ESMTP banner detected
- Domain identified:

```text
inlanefreight.htb
```

---

# 🧪 Initial Username Enumeration

## Nmap smtp-enum-users

```bash
nmap -p25 --script smtp-enum-users <TARGET-IP>
```

### Returned usernames

```text
root
admin
administrator
webadmin
sysadmin
netadmin
guest
user
web
test
```

---

# ⚠️ RCPT Enumeration

## Manual SMTP Testing

```bash
telnet <TARGET-IP> 25
```

```text
EHLO test.com
MAIL FROM:<test@test.com>

RCPT TO:<admin@inlanefreight.htb>
250 2.1.5 Ok

RCPT TO:<root@inlanefreight.htb>
250 2.1.5 Ok

RCPT TO:<random123@inlanefreight.htb>
250 2.1.5 Ok
```

---

## Analysis

The SMTP server accepted all recipient addresses.

This indicates a **catch-all configuration**, making RCPT enumeration unreliable.

---

# 🧠 VRFY Enumeration

## Stage 1 – Default Timeout

```bash
smtp-user-enum -M VRFY -U names.txt -t <TARGET-IP>
```

(Default timeout: `-w 5`)

### Result

```text
0 results
```

---

## Stage 2 – Increased Timeout

```bash
smtp-user-enum -M VRFY -U names.txt -t <TARGET-IP> -w 10
```

### Result

```text
0 results
```

---

## Stage 3 – Extended Timeout

```bash
smtp-user-enum -M VRFY -U names.txt -t <TARGET-IP> -w 15
```

### Result

```text
1 result found
```

---

# 🔬 Manual VRFY Verification

```text
VRFY root
252 2.0.0 root

VRFY mike
252 2.0.0 mike

VRFY admin
550 5.1.1 User unknown

VRFY random
550 5.1.1 User unknown
```

---

# 📊 Analysis

| Response | Meaning |
|----------|---------|
| 550 | Invalid user |
| 252 | Possible valid user (protected) |

The server implemented anti-enumeration behavior:
- invalid users returned `550`
- potential valid users returned `252`

---

# 🎯 Valid Username Identification

Two usernames returned `252`:
- root
- mike

`root` is a default system account and expected to exist.

`mike` is a non-default human-like account and was identified as the valid target user.

---

# ✅ Final Answer

```text
mike
```

---

# 🚀 Lessons Learned

- RCPT enumeration can fail because of catch-all behavior
- VRFY responses require manual interpretation
- SMTP servers may implement tarpitting and delayed responses
- Timeout tuning is critical during enumeration
- Generic wordlists are less effective than footprint-based lists
- Manual verification is essential when tool output appears inconsistent

---

# 🧰 Tools Used

- Nmap
- smtp-user-enum
- Telnet
- Netcat

---

# 🔐 Defensive Recommendations

- Disable VRFY and EXPN commands
- Implement stricter anti-enumeration protections
- Avoid catch-all configurations
- Monitor repeated SMTP enumeration attempts
- Rate-limit suspicious SMTP activity

---

# 📚 Key Concepts Demonstrated

- SMTP enumeration
- RCPT validation
- VRFY analysis
- Catch-all detection
- Timeout tuning
- Manual protocol interaction
- Enumeration protection analysis
