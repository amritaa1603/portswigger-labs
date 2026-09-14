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