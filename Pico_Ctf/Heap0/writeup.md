#HEAP 0 — PicoCTF 
---
PicoCTF practice problems :

https://play.picoctf.org/practice?page=3


### Description

Are overflows just a stack concern?Download the binary [here](https://artifacts.picoctf.net/c_tethys/15/chall).Download the source [here](https://artifacts.picoctf.net/c_tethys/15/chall.c).Connect with the challenge instance here:`nc tethys.picoctf.net 49941`

[here](https://artifacts.picoctf.net/c_tethys/15/chall)

[here](https://artifacts.picoctf.net/c_tethys/15/chall.c)

# Heap0 — Writeup (clean version)

**Target:** `nc tethys.picoctf.net 49941`

**Goal:** get the flag.

## What the program shows

After connecting the program prints a short menu and a “Heap State” that lists two heap addresses and their contents, for example:

```
0x...b2b0  -> pico
0x...b2d0  -> bico

1. Print Heap
2. Write to buffer
3. Print safe_var
4. Print Flag
5. Exit

```

From this output we immediately see:

- Two heap blocks exist and are printed (addresses + contents).
- One block contains `bico` (the program calls it `safe_var`).
- There is an option to **write** to a buffer (your block).
- There is an option to **print the flag**, but it only prints under some condition.

## Key observation / hint

The program also gives a textual hint in the code/menu: it lets you view `safe_var` and says it “can’t be modified” — that suggests the author expects `safe_var` to be protected, but also hints this is the thing you must change to win.

## Reading the source (important line)

Inside the source there is a check like:

```c
if (strcmp(safe_var, "bico") != 0) {
    printf("\nYOU WIN\n");
    // open flag file and print it
}

```

This tells us: **the flag is printed only if `safe_var` no longer equals `"bico"`**.

## Exploit idea (the trick)

- The writable buffer and `safe_var` are allocated *adjacent* on the heap (their addresses are close).
- If the program reads input into your buffer **without checking length**, writing more bytes than your buffer can hold will **overflow into the next heap block** and overwrite `safe_var`.
- So the plan is: use menu option 2 to write a long string (longer than the buffer), overflow `safe_var`, then use option 4 to trigger the `strcmp` check — which now passes — and the program will print the flag.

## Steps I executed (interactive)

1. `nc tethys.picoctf.net 49941` (connect)
2. Option `1` to print heap and note contents and addresses.
3. Option `3` to confirm `safe_var = bico`.
4. Option `2` and input a long string like:
    
    ```
    AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    
    ```
    
    (many `A`s; longer than the buffer)
    
5. Option `3` again → now `safe_var` shows the overflowed data (your `A`s).
6. Option `4` → program prints:
    
    ```
    YOU WIN
    picoCTF{Flag}
    
    ```
    

## Why this works (concise logic)

- `malloc` returned two nearby heap blocks.
- The program used unsafe input (no length check) to write into the first block.
- Excess bytes wrote into the neighboring block (`safe_var`), changing it from `"bico"` to your data.
- The check `strcmp(safe_var, "bico") != 0` became true, so the program printed the flag.

## What I learned

- Heap blocks can be adjacent; overflowing one can modify another.
- Input length checks are critical; missing them cause real vulnerabilities.
- CTFs often intentionally expose memory layout to teach exploitation concepts.


  **⚠️ Safety note**
  --
  
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.
