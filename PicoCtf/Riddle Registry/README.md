#Riddle Registry — PicoCTF 
---
PicoCTF practice problems :

https://play.picoctf.org/practice?page=1

Description
-
You found a PDF file that looks like random text or garbage. Don’t worry, the pages themselves don’t matter. The flag is hidden inside the PDF metadata, like the Author, Title, or other custom fields.
File:
Download confidential.pdf

---


What this means
--

The PDF pages may look like nonsense—ignore them.

The flag is stored in the PDF metadata (extra info about the file).

Metadata can include Author, Title, Creation Date, and custom fields.

Your task is to open the metadata and find the flag. You don’t need to read the pages.



---


How to check PDF metadata
--
Option 1: Using pdfinfo (Linux / Mac / Windows WSL)

Open terminal or command prompt.

Run:

pdfinfo filename.pdf


Look for lines like:

Author:          cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=




---

Option 2: Using exiftool

Open terminal.

Run:

exiftool filename.pdf


Check all metadata fields for suspicious strings.


---


Option 3: Using a PDF viewer

Right-click the PDF → Properties → Details (or open in PDF reader → Document Properties).

Check fields like Author, Title, or custom metadata.

Steps to decode

Copy the suspicious string (looks long and ends with =), for example:

cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=


---


Decode it using Python:
-

import base64

encoded = "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0="
decoded = base64.b64decode(encoded).decode()
print(decoded)

Or decode using command line (Linux / Mac / Git Bash):

echo "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=" | base64 -d

The output is the flag (do not share the full flag publicly if you post the write-up).



---

Notes & Tips
-

Sometimes metadata is in Author, Title, or custom fields—check all.

If the string looks encoded, try base64 first (common in CTFs).

Always redact the flag when sharing publicly (use FLAG{REDACTED}).



---

⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
