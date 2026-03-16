# Neighbour - TryHackMe

**Date:** 16.03.2026
**Difficulty:** Easy
**Category:** Web Exploitation
**Vulnerability:** IDOR (Insecure Direct Object Reference)

---

## Enumeration

Navigated to the target IP in the browser, which presented a login page.
Inspected the page source code and discovered a comment revealing that
guest credentials were available for users without an account.

---

## Exploitation

Logged in using the guest credentials found in the source code.
After authentication, the application redirected to:

`http://<target-ip>/profile.php?user=guest`

Noticing the `user` parameter in the URL, I attempted to access the
admin profile by modifying the parameter directly:

`http://<target-ip>/profile.php?user=admin`

The application returned the admin profile page without any
authorization check, exposing the flag.

---

## Flags

| Flag | Value |
|------|-------|
| User flag | THM{66be95c478473d91a5358f2440c7af1f} |

---

## Vulnerability Summary

The application failed to implement server-side access control on the
`profile.php` endpoint. The `user` parameter was trusted client-side,
allowing any authenticated user to access arbitrary profiles by simply
modifying the URL parameter. This is a textbook example of IDOR.

---

## Remediation

- Validate user identity server-side on every request
- Never rely on client-side parameters to determine access level
- Implement proper session-based authorization checks
