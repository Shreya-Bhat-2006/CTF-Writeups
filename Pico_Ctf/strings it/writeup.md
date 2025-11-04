# **strings it**

### Description

Can you find the flag in [file](https://jupiter.challenges.picoctf.org/static/5bd86036f013ac3b9c958499adf3e2e2/strings) without running it?

## HOW I  SOLVED IT ???

First, I checked what type of file it was using the `file` command.

It showed that the file was an **ELF 64-bit executable** and **not stripped**, which means it still contains readable strings inside it.

Since I didn’t want to run the file, I used **static analysis**.

I ran the `strings` command to extract all the human-readable text inside the binary:

```
strings -a strings

```

Then, I filtered that output to only show the lines that might contain a flag.

To do that, I used `grep` and searched for common CTF keywords like `flag`, `ctf`, and `thic`:

```
strings -a strings | grep -i -E "flag|ctf|thic"

```

This showed me several strings, and among them, one was clearly in the standard CTF flag format:

```
picoCTF{Flag}

```

So I didn’t need to run the program — I found the flag just by inspecting the readable strings stored inside the binary.

---

## ⭐ One-Line Summary

> I analyzed the binary statically using strings and filtered the output with grep to find the flag without executing the file.
> 

The command was:

```
strings -a strings | grep -i -E "flag|ctf|thic"

```

Think of this as **two tools connected together**:

---

### **1) `strings -a strings`**

- `strings` = a program that extracts **printable text** from a binary file.
- `a` = “search the **entire file**,” not just certain sections.
- The second `strings` is the **file name** you were analyzing.

So:

```
strings -a strings

```

means:

> “Show me all human-readable text hidden inside the file named strings.”
> 

---

### **2) `|` (the pipe symbol)**

This means:

> “Take the output of the left command and send it into the command on the right.”
> 

So we take the text extracted by `strings` and **feed it** into `grep`.

---

### **3) `grep -i -E "flag|ctf|thic"`**

`grep` is a **search tool**.

- `i` = ignore case (FLAG = flag = FlaG → all match)
- `E` = allow use of `|` (OR)
- `"flag|ctf|thic"` = words we are searching for

So it means:

> Show me lines that contain flag OR ctf OR thic (case doesn’t matter).
> 

## **What does `E` mean in `grep`?**

- `E` means:

> Use Extended Regular Expressions.
> 

Don’t worry — that sounds big, but it’s actually simple.

It just means:

### **You are allowed to use `|` to mean OR.**

---

## Without `E`

This would **NOT** work:

```
grep "flag|ctf|thic"

```

Because without `-E`, grep thinks `|` is a normal character, **not OR**.

---

## With `E`

This **does** work:

```
grep -E "flag|ctf|thic"

```

Now `|` means:

```
flag  OR  ctf  OR  thic

```
