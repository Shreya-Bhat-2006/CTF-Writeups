# **Glory of the Garden**

### Description

This [garden](https://jupiter.challenges.picoctf.org/static/d0e1ffb10fc0017c6a82c57900f3ffe3/garden.jpg) contains more than it seems.

### Meaning of the hint:

- The image **looks normal**, but **there is something hidden INSIDE it**.
- So we need to check the **raw data** of the image, not just the picture.

# How  I  Solved It

## Step 1 — Check metadata

I ran:

```
exiftool garden.jpg

```

This shows camera info, file timestamps and other metadata. Nothing suspicious was there, so the flag wasn’t stored in the metadata.

---

## Step 2 — Check for embedded files

I ran:

```
binwalk garden.jpg

```

Binwalk would have shown if there was a ZIP, PNG, or other file embedded inside the JPEG. It only showed standard JPEG data, so there were no embedded archives.

---

## Step 3 — Look inside the raw bytes

I ran:

```
strings garden.jpg | grep -i ctf

```

`strings` extracts readable text from a binary file, and `grep -i ctf` filters lines that mention “ctf”. That returned:

```
Here is a flag "picoCTF{Flag}"

```

So the flag was literally inside the JPEG bytes as plain text.

---

## Why this worked

This challenge used a simple stego trick: the author put the flag text inside the file’s binary data. The image still displays normally, but tools that read raw bytes (like `strings`) can show hidden readable text. This matched the hint: the garden (image) contains more than it seems.

---

## Quick logic summary

1. `exiftool` → checked metadata → nothing.
2. `binwalk` → checked for embedded files → nothing.
3. `strings` → searched raw bytes → **flag found**.

---

## One-line wrap-up

I found the flag because it was stored as plain text inside the JPEG binary, and strings revealed it.

