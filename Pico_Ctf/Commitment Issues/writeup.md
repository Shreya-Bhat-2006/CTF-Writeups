# **Commitment Issues**


PicoCTF practice problems :

https://play.picoctf.org/practice?page=3
---

### Description

I accidentally wrote the flag down. Good thing I deleted it!You download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/76/challenge.zip)

You’re given a ZIP containing a Git repository (`.git`) and a `message.txt`. The challenge says the flag was written down and then “deleted”. Your job: recover the deleted flag from the repo.

---

## the challenge in simple words

- A Git repository stores every change you make (commits).
- Even if somebody *deletes* a file later, Git still keeps the earlier commits where that file existed.
- The challenge hides the flag in a previous commit and then removes it in a later commit. So we just need to look at the repository **history** to find the older commit that created the flag.

Think of it like a stack of snapshots — deleting a file only affects the latest snapshot, not the old ones. We just look back in time.

## ✅ **How I Solved It   :**

First, I saw that the challenge folder contained a `.git` directory. This means the project was tracked using **Git**, and Git keeps **history** of all previous versions. So even if someone deleted the flag later, the older version might still have it.

### **1. I checked the commit history**

I used this command to see the list of commits:

```
git log --all --oneline

```

The output showed:

```
a6dca68 (HEAD -> master) remove sensitive info
e720dc2 create flag

```

From this, I understood:

- The commit **`a6dca68`** is the latest commit and it says **"remove sensitive info"** → meaning this is where the flag got deleted.
- The commit **`e720dc2`** (the one *before* it) says **"create flag"** → meaning the flag was originally added here.

So the flag should be present in commit `e720dc2`.

---

### **2. I viewed that earlier commit to see the deleted file**

I ran:

```
git show e720dc2

```

This showed the patch (the changes made in that commit), and I saw this:

```
+ picoCTF{Flag}

```

This line means the file `message.txt` was **created** in that commit and inside it was the **flag**.

---

## 🎓 What I learned

- Even if a file is deleted later, Git **does not erase** the past — I can always look at older commits.
- `git log --all --oneline` helps me quickly understand what happened in each commit.
- `git show <commit>` lets me see exactly what changed in that commit.
- I can recover deleted files using `git checkout <commit> -- <file>`.



⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
