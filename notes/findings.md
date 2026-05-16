# Findings

## SMTP Service

- SMTP exposed on port 25
- ESMTP enabled
- Domain identified:
  inlanefreight.htb

---

## Catch-All Behavior

RCPT enumeration returned:
- 250 for valid users
- 250 for random users

This confirmed:
- catch-all configuration
- unreliable RCPT validation

---

## VRFY Behavior

Two usernames produced different responses:

- root
- mike

Response:
252 2.0.0

All other users returned:
550 User unknown

---

## Valid Username

The identified valid user account was:

mike
