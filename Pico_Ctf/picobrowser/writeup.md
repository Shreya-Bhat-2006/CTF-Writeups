# **picobrowser**

### Description



This website can be rendered only by **picobrowser**, go and catch the flag! `https://jupiter.challenges.picoctf.org/problem/28921/` ([link](https://jupiter.challenges.picoctf.org/problem/28921/)) or http://jupiter.challenges.picoctf.org:28921




-----


When I opened the link, there was a page with a **flag button**, but when I clicked the flag, **nothing happened**. Then I read the question carefully — it said that *“This website can be rendered only by picobrowser.”*

From that, I understood that every time our browser asks a website for a page, it sends a message like this (a simplified version):

```
GET /flag HTTP/1.1
Host: jupiter.challenges.picoctf.org
User-Agent: Mozilla/5.0 (Windows NT 10.0; ...)
Accept: text/html
(other headers...)

```

So, I realized that I just need to **change the User-Agent** to `picobrowser`.

Then I right-clicked on the page and opened **Inspect**.

After that, I went to the **Network** tab.

At the top-right corner, I clicked on the **three dots (...)**, selected **More tools → Network conditions**.

There, I saw an option to set the **User-Agent** manually.

I unchecked “Use browser default” and changed the User-Agent to **picobrowser**.

Then I refreshed the page and clicked the **flag button** again — this time, it **showed me the flag**!


---

### 1. What actually happens when you open a website

When you open a website, your browser (like Chrome, Firefox, etc.) **sends a request** to the website’s server.

That request is called an **HTTP request**, and it contains a bunch of **headers** — small pieces of info about you and your browser.

One of these headers is:

```
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/142.0.0.0 Safari/537.36

```

This line basically **tells the website what browser and OS you are using** — for example, Chrome on Windows 10.

So the website can adjust how it behaves or looks based on what browser you are using.

For example:

- A site might look different on a mobile browser vs desktop.
- Some APIs or features only work in specific browsers.



### 2. What the challenge did

In this PicoCTF challenge, the website had a **condition** in its backend code something like this:

```python
if request.headers["User-Agent"] == "picobrowser":
    show_flag()
else:
    show_normal_page()

```

That means the **server checks the User-Agent** in your request — and only if it says **“picobrowser”**, it gives the flag.

But when you opened the page normally, your request had:

```
User-Agent: Mozilla/5.0 (...)

```

So the website thought you were using a normal browser, not "picobrowser", and therefore **hid the flag**.



---

⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.

