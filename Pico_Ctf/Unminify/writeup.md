# **Unminify**


PicoCTF practice problems :

https://play.picoctf.org/practice?page=3

## Description

I don't like scrolling down to read the code of my website, so I've squished it. As a bonus, my pages load faster!Browse [here](http://titan.picoctf.net:49884/), and find the flag!

## Solution :

When I opened the link, it was showing a normal message.

So I inspected the page by pressing **Ctrl + U** to view the source code.

Inside the source code, the **flag was already there**,

but it **wasn't showing on the website**.

The reason is:

The flag was written like this:

```html
<p class="picoCTF{Flag}"></p>

```

Here, `<p>` tag normally displays text.

**But they put the flag inside the `class` attribute**, not between the `<p>` and `</p>`.

Since **class is only used for styling** and does **not get displayed**,

the flag stays hidden on the page even though it exists in the source code.

So the flag is there, but it's **not visible on screen**, only visible when we check the **HTML code**.

---

### ⭐ One-Line Conclusion

**The flag was hidden inside the `class` attribute, so it didn’t show on the webpage, but we found it in the source.**




⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
