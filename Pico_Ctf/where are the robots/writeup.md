# **where are the robots**

### Description

Can you find the robots? `https://jupiter.challenges.picoctf.org/problem/36474/` ([link](https://jupiter.challenges.picoctf.org/problem/36474/)) or http://jupiter.challenges.picoctf.org:36474

## HOW I  SOLVED

When I opened the challenge page it just said:

```
Welcome
Where are the robots?

```

At first I didn’t know what “robots” meant, so I inspected the page source — nothing suspicious there. Then I searched what “robots” could mean and learned about `robots.txt`. It’s a small text file on websites that tells search engines which pages not to index. It’s always at the site root, like `https://example.com/robots.txt`.

The file looks like this:

```
User-agent: *
Disallow: /secret/

```

That means “all bots, don’t index `/secret/`.” But `robots.txt` only tells bots what to ignore — it doesn’t stop humans from visiting those pages. So CTFs often hide secret pages there on purpose.

I added `/robots.txt` to the challenge URL:

```
http://jupiter.challenges.picoctf.org:36474/robots.txt

```

It showed:

```
User-agent: *
Disallow: /477ce.html

```

Then I opened:

```
http://jupiter.challenges.picoctf.org:36474/477ce.html

```

And that page contained the flag. That’s how I solved it.


⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.

