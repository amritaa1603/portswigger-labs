## Username Enumeration via Different Responses
**Category:** Authentication · **Difficulty:** Apprentice · **Status:** ✅ Solved

**What is this?**
The login form shows a different error for a wrong username than for a wrong password, so
you can figure out which usernames actually exist before touching passwords at all.

**How I solved it**
1. Logged in with random credentials and got "invalid username" as the error.
2. Sent the /login request to Intruder, marked the username field, and pasted the username list.
3. After the attack finished, looked for a response with a different length — found one username that stood out.
4. Replaced username with that confirmed one, marked the password field, and pasted the password list.
5. After that attack, again checked for a different length and got the matching password.
6. Logged in with that username and password and got into the account.

**What I learned**
Error messages don't need to say "user exists" directly — a different response length or
wording is enough to leak it. Fix is making both failure cases return the exact same response.

---
## 2FA Simple Bypass
**Category:** Authentication · **Difficulty:** Apprentice · **Status:** ✅ Solved

**What is this?**
After a correct password, the app sends you to a 2FA code page — but it doesn't actually check
server-side whether that step was completed before letting you into the account.

**How I solved it**
1. Logged in with the target user's known username and password.
2. Got redirected to the 2FA code entry page as expected.
3. Instead of entering a code, navigated straight to /my-account manually.
4. The page loaded normally, no code check enforced at all.

**What I learned**
2FA is only real if the server tracks "has this session actually passed step 2" — if it's just
a page in the flow, skipping it skips the whole check.

---
## Password Reset Broken Logic
**Category:** Authentication · **Difficulty:** Apprentice · **Status:** ✅ Solved

**What is this?**
The password reset link has a token and a username field, but the two aren't properly tied
together server-side — so a valid token from one account can be reused for another.

**How I solved it**
1. Triggered a password reset for my own account to get a working, valid token.
2. Intercepted the final reset request in Burp before submitting it.
3. Kept my own valid token but changed the `username` field to the target user.
4. Set a new password of my choosing and sent the request.
5. Server accepted it and reset the target's password instead of mine.

**What I learned**
Any flow using two separate identifiers (token + username) needs to check they actually belong
together — otherwise you're really only validating one of them.

---
## Username Enumeration via Subtly Different Responses
**Category:** Authentication · **Difficulty:** Practitioner · **Status:** ✅ Solved

**What is this?**
Same idea as regular username enumeration, except the error text looks identical either way —
the difference is hidden in something smaller, like response length.

**How I solved it**
1. Ran the username wordlist through Intruder with a fixed wrong password.
2. Sorted results by response length instead of reading the error text.
3. Found one entry a few bytes different from all the rest — that was the valid username.
4. Repeated the same process with a password wordlist against that username.
5. Sorted by length again and found the successful login attempt.

**What I learned**
"Looks the same" isn't "is the same" — always check raw response length and status code, not
just what the error message says on screen.

---