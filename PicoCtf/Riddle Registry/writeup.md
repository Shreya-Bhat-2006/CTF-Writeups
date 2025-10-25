Riddle Registry — PicoCTF 2025



Problem statement:

You are given a PDF file that looks like random or garbled text. The pages themselves are not important — the flag is hidden in the PDF metadata (fields like Author, Title, or other custom tags). Find the PDF and extract the flag from its metadata.



File:

https://challenge-files.picoctf.net/c\_saffron\_estate/8175871c08fc4d10d26d10746f3a4aad06bbd7a5682c01c3d777a89d276bdb38/confidential.pdf



What this means:

\- The readable pages may look like nonsense — ignore them.

\- Look at the file properties / metadata (Author, Title, etc.).

\- The flag is encoded inside one of those metadata fields (often base64).

\- You only need to inspect and decode the metadata to get the flag.



Step-by-step solution:



1\. Download the PDF

Save confidential.pdf to your working folder.



2\. Inspect metadata (quick options)



Option A — pdfinfo (Linux / macOS / Windows WSL)

pdfinfo confidential.pdf

Look for lines like Author: ... or Title: ...



Option B — exiftool

exiftool confidential.pdf

This shows all metadata fields.



Option C — Use a PDF viewer’s Properties

Right-click the file → Properties (or open in a PDF reader and check Document Properties) and look at Author/Title/custom fields.



3\. Spot the suspicious string

If you see a long, random string (often ending with =), it is probably base64.

Example (what you might see):

Author: cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=

Do not post the decoded flag publicly. Show proof by redacting the flag (FLAG{REDACTED}) or showing a truncated form.



4\. Decode the base64 string



Method A — Python

import base64

s = "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0="

print(base64.b64decode(s).decode())



Method B — Command line (Linux / macOS / Git Bash)

echo "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV9lZTQ1NDk1MH0=" | base64 -d



The decoded output is the flag (do not paste the full flag publicly).



Problems faced:

\- Metadata might be in different fields (Author, Title, or custom). Check all.

\- Some viewers hide custom metadata — use exiftool or pdfinfo to be sure.



Notes \& tips:

\- The PDF producer PyPDF2 is common and not an issue.

\- If metadata looks encrypted/obfuscated, try common encodings first (base64, hex).

\- Always redact the final flag when sharing the writeup publicly.



Proof:

I show the metadata string I found (as base64) and explain the decode step. I replace the actual decoded flag with FLAG{REDACTED} when I publish the writeup.



