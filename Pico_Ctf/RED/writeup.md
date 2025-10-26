# 🟥 PicoCTF — RED, RED, RED, RED

---

## Description

There is a file `red.png`. The challenge name is **RED, RED, RED, RED**.

**Hint given:**  
Red?Ged?Bed?Aed? Check whatever Facebook is called now.

**Goal:**  
Find the hidden flag inside `red.png` without posting it publicly.

---

## Steps I Followed

1. **Look at the image**  
   The image is just a red square. Nothing visible.

2. **Check metadata**  
   I ran:
   ```bash
   exiftool red.png
Found a Poem field (some lines of text). At first, it looked normal.
--

Crimson heart, vibrant and bold,
Hearts flutter at your sight.
Evenings glow softly red,
Cherries burst with sweet life.
Kisses linger with your warmth.
Love deep as merlot.
Scarlet leaves falling softly,
Bold in every stroke.

---

3. **Read the first letters**

I took the first letter of every line in the poem.

The letters spelled:  CHECK LSB 

***Hint: check the Least Significant Bits of the image pixels.***

---
4. **Run stego tool**
I used zsteg to automatically search LSBs:

**zsteg red.png**

zsteg found some hidden text in the image LSBs.
The hidden text looked like Base64 (ended with ==).

---

Decode Base64
I copied the Base64 string the tool gave and decoded it locally:

**echo "BASE64_STRING_HERE" | base64 --decode**

That gave the final flag text (not posted here).

---

**Commands Summary :**
 ```bash
exiftool red.png        # check metadata
zsteg red.png           # search LSBs and bitplanes (PNG stego)
 ```

 if you get a base64 string from zsteg, decode it locally:
--
  ```bash
echo "PASTE_BASE64_HERE" | base64 --decode
```

---
Why This Worked (In Simple Words)
--
The image looked normal, but CTFs hide stuff inside files.

The poem was a clue — its first letters told me to check LSB.

LSB steganography stores hidden data in the smallest bits of pixel values.

zsteg reads those bits and shows hidden text.

The hidden text was Base64, so decoding it gave the flag.
-  --
Notes / Tips
--
Do not post the flag publicly — follow the rules.

If zsteg is not installed, you can try a Python script that reads the red-channel LSBs and combines bits to make bytes.

The hint “whatever Facebook is called now” (Meta) pointed to metadata, so always check metadata first.


---

⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
