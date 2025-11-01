# **Secret of the Polyglot**



PicoCTF practice problems :

https://play.picoctf.org/practice?page=3
---


### Description

The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file?Download the suspicious file [here](https://artifacts.picoctf.net/c_titan/96/flag2of2-final.pdf).

[here](https://artifacts.picoctf.net/c_titan/96/flag2of2-final.pdf)

1. I saw a file named `flag.pdf` and opened it, but the text I found looked like **not the full flag**.
2. The problem hint said: **“open the file in different ways.”** That made me think the file might hide more than one thing.
3. I ran `binwalk flag.pdf` to check the file contents. It showed: `PNG` at the start, then a `PDF`, then compressed data.
4. From that I understood: the file is **both a PNG and a PDF** (two files in one).
5. I made a copy and changed its name to `flag.png` using `cp flag.pdf flag.png`. This only changed the name, not the file data.
6. I opened `flag.png` (Windows Photos). The image showed some text — **this was the first half of the flag**.
7. I also opened the original `flag.pdf` with a PDF reader. That showed the other text — **the second half of the flag**.
8. I read the two parts carefully and joined them together in order.
9. The full flag became: `picoCTF{Flag}`.
10. I submitted the combined flag and it worked.



⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
