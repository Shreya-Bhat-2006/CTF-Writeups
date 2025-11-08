# **flag_shop**

### Description

There's a flag shop selling stuff, can you buy a flag? [Source](https://jupiter.challenges.picoctf.org/static/64e724ad327f83ad833d9c6baa072b1f/store.c). Connect with `nc jupiter.challenges.picoctf.org 4906`.

## How I Solved

- I got the source code and the server address: `nc jupiter.challenges.picoctf.org 4906`.
- I connected with `nc` and saw the menu:
    - `1. Check Account Balance`
    - `2. Buy Flags`
    - `3. Exit`
- I chose option `1` and it showed my balance: **1100**.
- I chose option `2` to buy flags. It showed two items:
    - `1. Definitely not the flag Flag`
    - `2. 1337 Flag`
- I inspected the source code and found the 1337 flag only opens `flag.txt` if `account_balance > 100000`. But my balance is 1100, so normally I can’t buy it.
- In the “Definitely not the flag” option the code does this:
    - `total_cost = 900 * number_flags;`
    - `account_balance = account_balance - total_cost;`
- Because `int` is a signed 32‑bit on the server, if `900 * number_flags` is bigger than `INT_MAX` (2,147,483,647) it **overflows** and becomes a **negative** number (wraps around).
- If `total_cost` becomes negative, `account_balance - total_cost` actually **adds** that absolute value to the balance — so my balance increases a lot.
- The smallest number that causes overflow is `2386093` (because `900 * 2386093 = 2,147,483,700` which is just over `INT_MAX`).
- So I did: buy the knockoff flags and entered `2386093` (or any large overflowing number). After that the program made `total_cost` negative and my `account_balance` became huge (much more than 100000).
- Then I went back to Buy Flags → chose option `2` (1337 Flag) and entered `1` to buy. Now `if (account_balance > 100000)` is true, the program opened `flag.txt` and printed the flag. 
