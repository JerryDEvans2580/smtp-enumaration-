# Lessons Learned

## 1. RCPT Enumeration Is Not Always Reliable

SMTP catch-all configurations can invalidate RCPT enumeration results.

Always test random usernames to confirm behavior.

---

## 2. Manual Verification Is Critical

Automated tools may miss valid results or incorrectly interpret SMTP responses.

Manual analysis is essential.

---

## 3. Timeout Tuning Matters

Default timeout values may fail against protected SMTP services.

Increasing:
-w 5 → -w 10 → -w 15

allowed meaningful responses to be captured.

---

## 4. Anti-Enumeration Protections Exist

SMTP services may:
- delay responses
- tarpitting connections
- hide valid users behind 252 responses

---

## 5. Targeted Wordlists Are Better

Generic wordlists are less effective than footprint-based wordlists built from:
- banners
- domains
- discovered names
- environmental context
