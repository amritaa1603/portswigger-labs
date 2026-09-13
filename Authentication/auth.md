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