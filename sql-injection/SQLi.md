# PortSwigger Web Security Academy — Lab Write-ups

Personal write-ups for labs I've solved on PortSwigger's Web Security Academy, written the way
I actually worked through them — what the bug was, how I found and exploited it, and what I
took away from it.

---

## 1. SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data
**Category:** SQL Injection · **Difficulty:** Apprentice · **Status:** ✅ Solved

**What is this?**
A product filter passes the `category` value straight into a SQL `WHERE` clause with no
sanitization, so whatever I type becomes part of the actual query.

**How I solved it**
1. Tried a single quote (`'`) in the category param and got a database error back.
2. That confirmed the input wasn't sanitized, so I could break out of the string.
3. Sent the payload below to close the original query and inject an always-true condition.
4. Response came back with every product listed, including hidden/unreleased ones.

```
/filter?category=Gifts'+OR+1=1--
```

**What I learned**
A single broken quote is usually enough to prove SQLi exists, and `OR 1=1` is the fastest way
to confirm you control the query logic. Real fix is parameterized queries, never string
concatenation.

---