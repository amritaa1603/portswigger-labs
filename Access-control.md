## Unprotected Admin Functionality
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
The admin panel isn't linked anywhere in the site's normal navigation, but it also isn't
actually protected by any login or role check — it's only hidden by obscurity.
 
**How I solved it**
1. Checked `/robots.txt` on the site, since unlinked admin pages often get disallowed there so search engines skip them.
2. Found a disallowed path pointing to an admin panel URL.
3. Navigated to that URL directly in the browser, with no login at all.
4. Page loaded straight into the admin panel with full functionality available.
5. Used the "delete user" option on the panel to delete the target account and solve the lab.
**What I learned**
Hiding a URL isn't access control — `robots.txt` is public by design and basically hands out a
map of "pages we don't want indexed," which often means "pages we forgot to protect." Real
admin panels need a proper server-side role check on every request, not just absence from the nav menu.
---

## Unprotected Admin Functionality with Unpredictable URL
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
The admin panel URL this time isn't guessable or in `robots.txt` — but the page still leaks it somewhere else in the site's front-end code, since there's no real login check protecting it.
 
**How I solved it**
1. Checked `robots.txt` first out of habit, but it had nothing useful this time.
2. Opened the site's page source and looked through the linked JavaScript files.
3. Found a script that referenced the actual admin panel path directly in its code.
4. Navigated to that unpredictable URL manually — no login prompt appeared.
5. Used the panel's "delete user" option to solve the lab.
**What I learned**
An unguessable URL isn't protection if it's referenced anywhere in client-side code — JS files, page source, and network requests are all public once they reach the browser.
 
---
##  User Role Controlled by Request Parameter
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
The app decides whether you're an admin based on a value sent from the client — in this case a cookie — instead of checking anything server-side that the user can't touch.
 
**How I solved it**
1. Logged in with a normal low-privilege account and opened Burp to inspect the traffic.
2. Noticed a cookie in the request, something like `Admin=false`.
3. Intercepted a request and flipped that value to `Admin=true`.
4. Forwarded the request and reloaded the account page.
5. Admin panel access appeared, and the lab was solved from there.
**What I learned**
Never trust a role/permission flag that's stored on the client side, even in something as ordinary-looking as a cookie — the server has to be the one deciding who's an admin, every time.
 
---
 
##  User Role Can Be Modified in User Profile
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
The "update profile" form on the account page silently accepts a `roleid` parameter in its request, even though the visible form doesn't expose that field to the user at all.
 
**How I solved it**
1. Opened my account/profile page and sent a normal profile update through Burp.
2. Looked at the raw request body and spotted a `roleid` parameter tied to my account.
3. Changed that value to the ID that corresponds to the admin role.
4. Forwarded the modified request.
5. Logged back into the account and found admin access was granted.
**What I learned**
Hidden or "internal" form fields aren't actually hidden from someone intercepting the request — if the server accepts a parameter without re-checking permissions, the UI not showing it means nothing.
 
---
 
##  User ID Controlled by Request Parameter
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
The account page loads user data based on an `id` parameter in the URL, and the server doesn't check whether that ID actually belongs to the logged-in user.
 
**How I solved it**
1. Logged in normally and noticed the account page URL included my own `id` value.
2. Swapped that `id` for the target user's known username/ID in the URL.
3. Reloaded the page.
4. The target user's account information loaded directly, with no ownership check.
**What I learned**
This is a textbook IDOR — any time an ID in a URL or parameter controls which record you see, the server needs to verify that record actually belongs to the requester, not just that the ID is valid.
 
---