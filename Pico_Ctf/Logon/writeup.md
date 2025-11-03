# **logon**

### Description

The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? `https://jupiter.challenges.picoctf.org/problem/44573/` ([link](https://jupiter.challenges.picoctf.org/problem/44573/)) or http://jupiter.challenges.picoctf.org:44573

## What I Did

When I opened the website, it asked for a username and password. I tried the username **joe** and left the password blank, and it still logged me in. However, the page said **“No flag for you.”**

So I inspected the page using the browser tools. Under **Application → Cookies**, I saw a cookie named:

```
admin = False

```

This cookie is stored in the browser and the website uses it to decide if the user is an admin or not. Since it was `False`, the website didn’t show the flag.

I changed the value from:

```
admin=False

```

to

```
admin=True

```

and then refreshed the page.

**After reloading, the website thought I was an admin, and now it showed the flag.**

### **Logic:**

The website was trusting the cookie value (`admin=True/False`) to decide permissions. Since cookies are stored on the client side (browser) and can be edited, changing it tricked the website into thinking I was an admin. So it displayed the flag.

---

### **One-Line Summary**

I changed `admin=False` to `admin=True` in cookies, and the website trusted that and showed me the flag. 




⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
