# **Blame Game**

### Pico ctf : https://play.picoctf.org/practice/challenge/405?page=4

### Description

Someone's commits seems to be preventing the program from working. Who is it?You can download the challenge files here:

- [challenge.zip](https://artifacts.picoctf.net/c_titan/157/challenge.zip)

## **What was the question actually asking?**

The question said:

> "Someone's commits seems to be preventing the program from working. Who is it?"
> 

This means:

- There is some **person** (developer) who changed the code.
- And **their change broke the program**.
- We need to **find who** did it.

So we don’t fix the program — we just find the **person responsible**.

## How i sloved it

When I downloaded the challenge zip file, I saw that the project had a `.git` folder and a `message.py` file. When I opened `message.py`, I noticed that there was a syntax error:

```python
print("Hello, World!"

```

The closing bracket `)` was missing, so the program was broken.

But the question was not asking me to fix the code — it was asking **who made the commit that caused this error**.

So I understood that I have to look through the **Git history** to find **which person last edited this file**.

To do that, I used the command:

```bash
git blame message.py

```

This command shows who wrote each line of the file.

The output showed:

```
2466febd (picoCTF{Flag} 2024-03-12) print("Hello, World!"

```

So, the **person** who made the commit that broke the code is:

```
picoCTF{Flag}

```

And that is the **flag** for the challenge. 


  **⚠️ Safety note**
  --
  
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
