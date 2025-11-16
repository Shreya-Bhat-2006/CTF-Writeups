# **Collaborative Development**

### Description

My team has been working very hard on new features for our flag printing program! I wonder how they'll work together?You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/69/challenge.zip)

### When I downloaded the ZIP file and extracted it, I saw a **drop-in** folder.

Inside that folder, there were:

- a **.git** folder
- a **flag.py** file

When I opened `flag.py`, it only had:

```python
print("Printing the flag...")

```

So the main file didn’t contain the flag.

---

# **Step 1 — Check available branches**

In the question, the hint said:

> “git branch -a will let you see available branches.”
> 

So I ran this command:

```
git branch -a

```

This showed 4 branches:

```
feature/part-1
feature/part-2
feature/part-3
main

```

This means the project has multiple branches, and maybe the flag is hidden in them.

---

# **Step 2 — Switch to part-1 branch**

I switched to the first branch using:

```
git checkout feature/part-1

```

This command changes my working directory to that branch.

Then I checked what is inside `flag.py` in that branch:

```
type flag.py

```

(`type` shows the contents of the file in Windows)

It printed the **first part of the flag**.

---

# **Step 3 — Switch to part-2 branch**

Same process:

```
git checkout feature/part-2
type flag.py

```

This printed the **second part of the flag**.

---

# **Step 4 — Switch to part-3 branch**

Again:

```
git checkout feature/part-3
type flag.py

```

This printed the **final part of the flag**.

---

# **Final Step — Combine all parts**

After collecting all three pieces from the three branches, I joined them to get the **full flag**.
