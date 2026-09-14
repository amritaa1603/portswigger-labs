# PortSwigger Web Security Academy — Writeups

Hands-on lab writeups documenting my web security learning journey.

## Progress

| Topic | Completed | Total |
|-------|-----------|-------|
| SQL Injection | 3 | 18 |
| XSS | 0 | 30 |
| Authentication | 7 | 14 |
| Access Control | 6 | 13 |
| IDOR | 0 | 13 |
| CSRF | 0 | 4 |

## Labs Solved

### SQL Injection
- SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
- SQL injection vulnerability allowing login bypass
- SQL injection UNION attack, determining the number of columns returned by the query

### Authentication
- Username enumeration via different responses
- 2FA simple bypass
- Password reset broken logic
- Username enumeration via subtly different responses
- 2FA broken logic
- Brute-forcing a stay-logged-in cookie
- Broken brute-force protection, IP block

### Access Control
- Unprotected admin functionality
- Unprotected admin functionality with unpredictable URL
- User role controlled by request parameter
- User role can be modified in user profile
- User ID controlled by request parameter
- User ID controlled by request parameter, with unpredictable user IDs

Full writeups: [all-labs-writeups.md](all-labs-writeups.md)

## Tools Used
- Burp Suite
- Browser DevTools

## Links
- [TryHackMe](https://tryhackme.com/p/Ash1603)
- [GitHub](https://github.com/amritaa1603)