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
## User ID Controlled by Request Parameter, with Unpredictable User IDs
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
Same IDOR pattern as before, except this time the user IDs are long random-looking values (like GUIDs) instead of simple numbers — so guessing one isn't realistic, the ID has to be found somewhere.
 
**How I solved it**
1. Confirmed the account page still worked the same way — whatever ID was in the URL decided whose data loaded.
2. Since brute-forcing a GUID wasn't practical, looked elsewhere on the site for a leaked ID.
3. Found the target user's ID exposed in another part of the app (a blog comment/author link) that wasn't protected.
4. Copied that ID into the account page's `id` parameter.
5. Page loaded the target user's data successfully.
**What I learned**
Making an identifier unpredictable only helps if it's never exposed anywhere else — one leaky endpoint (comments, avatars, public profile links) is enough to defeat the whole "unguessable ID" protection.
 
---
 
##  User ID Controlled by Request Parameter with Data Leakage in Redirect
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
Accessing another user's `id` correctly triggers a redirect to the login page (so it looks
protected), but the actual response body sent along with that redirect still contains the
target user's sensitive data before the browser ever follows it.
 
**How I solved it**
1. Logged in and grabbed a valid session cookie/request for the account page.
2. Sent a request with the `id` parameter swapped to the target user through Burp Repeater instead of a normal browser.
3. Got back a 302 redirect response to `/login`, which a browser would normally just follow silently.
4. Looked at the actual body of that 302 response instead of following it.
5. Found the target user's sensitive info (like their API key) sitting right there in the redirect response body.
**What I learned**
A redirect isn't the same as "access denied" — the server had already rendered the page and put sensitive data in the response before deciding to redirect. Anyone using a raw HTTP client (not a browser) can just read that body directly.
 
---
 
##  User ID Controlled by Request Parameter with Password Disclosure
**Category:** Access Control · **Difficulty:** Apprentice · **Status:** ✅ Solved
 
**What is this?**
The account page for another user loads with a pre-filled password field in the page's HTML — meant to be hidden/masked in the UI, but the actual value is sitting in the raw response.
 
**How I solved it**
1. Swapped the `id` parameter in the account page URL to the target user, same as the earlier IDOR labs.
2. Instead of just looking at the rendered page, viewed the raw HTML response.
3. Found an input field (something like `current-password`) that had the target's actual password pre-filled as its value.
4. Copied that password out of the HTML source.
5. Logged in as the target user using the leaked password to fully solve the lab.
**What I learned**
A masked/hidden input on screen only hides the value visually — the actual HTML sent to the browser can still contain the real data in plaintext. Never pre-fill sensitive fields server-side unless the value is meant to be exposed.