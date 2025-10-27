# Verify — picoCTF Writeup


picoctf problems :
https://play.picoctf.org/practice?page=2



**Short easy summary**

People were hiding real flags among many fake files. The challenge gives you a SHA-256 checksum (a long fingerprint) and a `decrypt.sh` script. You must: 1) find which file matches that checksum, 2) only then run the decrypt script on that verified file to get the real flag.

---

## Steps I followed (easy words + commands)

1. **Connect to the challenge machine**

```bash
ssh -p 54759 ctf-player@rhea.picoctf.net
# When asked: type `yes` to accept the fingerprint
# When asked for password: type `6dd28e9b` (you won't see characters while typing)
```

2. **See what files are there**

```bash
ls
# showed: checksum.txt  decrypt.sh  files

ls files
# many files like: 00011a60  4CwloraZ  ... (lots of decoys)
```

3. **Find which file matches the given SHA-256 checksum**

The challenge gave this checksum (trusted fingerprint):

```
03b52eabed517324828b9e09cbbf8a7b0911f348f76cf989ba6d51acede6d5d8
```

Compute SHA-256 for every file and look for that exact hash:

```bash
sha256sum files/* | grep 03b52eabed517324828b9e09cbbf8a7b0911f348f76cf989ba6d51acede6d5d8
```

**Output** (this means a match was found):

```
03b52eabed517324828b9e09cbbf8a7b0911f348f76cf989ba6d51acede6d5d8  files/00011a60
```

This proves `files/00011a60` is the real file (its fingerprint matches the trusted one).

4. **Run the decrypt script on the verified file**

Only after verifying the hash, run:

```bash
./decrypt.sh files/00011a60
```

**Output (the flag):**

```
picoCTF{trust_but_verify_00011a60}
```

---

## What each command means (very simple)

* `ssh -p 54759 ctf-player@rhea.picoctf.net`: connect to the remote challenge machine on port 54759.
* `ls` / `ls files`: list files so you can see what is available.
* `sha256sum files/*`: compute the SHA-256 fingerprint for each file in `files/`.
* `| grep <hash>`: filter the list to find the file whose fingerprint equals the trusted hash.
* `./decrypt.sh files/00011a60`: run the decrypt program on the verified file to reveal the flag.

---

## What is that long hash and why it was given?

* The long string (SHA-256) is a **fingerprint** of the real file. It’s a fixed 64-character signature computed from the file contents.
* If the file changes even a little, the fingerprint changes completely.
* The challenge gave this fingerprint so you can check which file is authentic among many fakes.

---

## Final checklist (copy-paste for teammates)

1. SSH into the server: `ssh -p 54759 ctf-player@rhea.picoctf.net` (password: `6dd28e9b`).
2. `ls` and `ls files` to see files.
3. Run: `sha256sum files/* | grep <given-hash>` and note the matching filename.
4. If a file matches, run: `./decrypt.sh files/<that-file>`.
5. Read the flag printed to the screen.

---

## Final note

Always verify a file with the checksum before running scripts on it. That way you avoid fake flags and potential bad files. This challenge demonstrates the security idea: **"trust, but verify."**

---

⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
