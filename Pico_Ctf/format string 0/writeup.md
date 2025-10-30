# format string 0 — picoCTF Writeup


picoctf problems :
https://play.picoctf.org/practice?page=3



----

### Description

Can you use your knowledge of format strings to make the customers happy?Download the binary [here](https://artifacts.picoctf.net/c_mimas/67/format-string-0).Download the source [here](https://artifacts.picoctf.net/c_mimas/67/format-string-0.c).Connect with the challenge instance here:

`nc mimas.picoctf.net 51892`

[here](https://artifacts.picoctf.net/c_mimas/67/format-string-0)

[here](https://artifacts.picoctf.net/c_mimas/67/format-string-0.c)

# 1) The normal, correct usage

```c
printf("Hello %s %x\n", name, some_int);

```

- The format string `"Hello %s %x\n"` contains **two** format specifiers: `%s` and `%x`.
- `printf` expects exactly two corresponding extra arguments after the format string:
    1. `name` for `%s` — `printf` treats this argument as a pointer to a C string and prints characters until `\0`.
    2. `some_int` for `%x` — `printf` treats this as an unsigned integer and prints it in hex.

So `printf` parses the format string, finds `%s` → pulls the first argument (`name`) → prints it as a string. Then finds `%x` → pulls the second argument (`some_int`) → prints it in hex. Everything lines up, so output is sensible.

---

# 2) The problematic case: no extra arguments

```c
printf("AAAA %x %x %x");

```

This format string has three `%x` specifiers, but **no** extra arguments were supplied. What happens?

- `printf` still parses the format string and for each `%x` it **tries to read one argument** from where it expects arguments to be.
- Since the caller didn't actually pass arguments, `printf` will read whatever happens to be in those argument slots (stack or register save area). Those slots contain unrelated bytes: return address, saved frame pointer, local variables, leftover register values, etc.
- The printed values are therefore **garbage** from memory — unpredictable, but often useful to an attacker because they reveal addresses/values.

Important: `AAAA`  at the start is plain text in the format string and will be printed exactly as `"AAAA "`. It's not involved in the argument list.

Example possible output:

```
AAAA 80485d3 7ffeefbff5c0 1

```

Each of the three hex numbers came from memory slots `printf` read as if they were arguments.

## 🔹 Step 1 — When I connected first time

I ran:

```
nc mimas.picoctf.net 51892

```

The program printed menu like:

```
Welcome to our newly-opened burger place...
Choose a burger:
Breakf@st_Burger
Gr%114d_Cheese
Bac0n_D3luxe

```

I got confused 🤨

So I opened the **source code** to understand what is going on.

---

## 🔹 Step 2 — Understanding the Flag Printing Logic

In source code I saw:

```c
signal(SIGSEGV, sigsegv_handler);

```

and

```c
void sigsegv_handler(int sig) {
    printf("\n%s\n", flag);
    exit(1);
}

```

So → If we somehow make the program **segfault (crash)** → The **handler will print the flag** 🎯

So the **main plan** = **Make program crash intentionally.**

---

## ✅ But there are 2 steps to reach crash

First we must **pass Patrick** (customer 1).

Then we get to **SpongeBob** (customer 2) where we can crash the program.

---

## 🔹 Step 3 — First Menu Logic (Patrick)

This part is in source:

```c
else {
    int count = printf(choice1);
    if (count > 2 * BUFSIZE) {
        serve_bob();
    }
}

```

- It prints **our input** using `printf(choice1)` → **format string vulnerability**
- Then `printf` returns how many characters were printed.
- If printed **count > 64**, then only we go to next stage (`serve_bob`).

So we must enter something that makes `printf` print **LOTS of characters**.

### The correct burger:

```
Gr%114d_Cheese

```

Break it:

| Part | Meaning |
| --- | --- |
| `Gr` | text |
| `%114d` | print a number in **field width 114** (→ prints MANY spaces) |
| `_Cheese` | text |

The program does:

```
printf("Gr%114d_Cheese");

```

But **no integer was provided**, so it prints **garbage random number** but still with *width 114 spaces*, like:

```
Gr                                                                                                           4202954_Cheese

```

So total length printed ≈ 123 characters → **123 > 64** → ✅ condition passed → **Next stage unlocked** ✔️

---

## 🔹 Step 4 — Second Menu Logic (SpongeBob)

Now menu contains:

```
Pe%to_Portobello
$outhwest_Burger
Cla%sic_Che%s%steak

```

This time code says:

```c
printf(choice2);

```

No length check. No arguments passed.

So we look for **format specifiers** inside menu options.

### The only dangerous one:

```
Cla%sic_Che%s%steak

```

This contains **three `%s`**.

### Why %s matters

`%s` tells printf:

> print a string from a memory address (pointer argument)
> 

But **we did NOT provide any pointer arguments**.

So printf reads **random addresses** → tries to access them → some addresses are invalid → **program crashes → SIGSEGV → FLAG prints.**

BOOM 💥

---

## 🔹 Step 5 — Final Input to Get Flag

So after first correct input, enter:

```
Cla%sic_Che%s%steak

```

Then program crashes → signal handler runs → **FLAG prints** 🎉




 

⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.

