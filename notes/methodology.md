# Methodology

## 1. Service Discovery

SMTP service discovery was performed using Nmap service detection.

Objectives:
- Identify SMTP service
- Extract banner information
- Discover target domain

---

## 2. Initial Enumeration

The smtp-enum-users NSE script was used to identify possible usernames.

The initial results returned multiple usernames, but required manual validation.

---

## 3. RCPT Verification

Manual SMTP interaction was performed using Telnet.

The RCPT TO command accepted both valid and random users.

This behavior revealed a catch-all SMTP configuration.

As a result:
- RCPT enumeration became unreliable
- Manual verification was required

---

## 4. VRFY Enumeration

The VRFY method was tested using smtp-user-enum.

Three timeout stages were used:
- -w 5
- -w 10
- -w 15

Only the extended timeout returned meaningful results.

---

## 5. Manual Analysis

SMTP responses were analyzed manually.

Key observation:
- Invalid users returned 550
- Potential valid users returned 252

This revealed anti-enumeration protections implemented by the SMTP service.
