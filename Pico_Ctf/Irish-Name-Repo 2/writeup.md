# **Irish-Name-Repo 2**

### Description

Someone has bypassed the login before, and now it's being strengthened. Try to see if you can still login!http://fickle-tempest.picoctf.net:53779

## exactly what I did ??

- **Opened the site and went to the login page.**
    
    I expected the flag to be shown after I logged in as `admin`, so I went to the login form.
    
- **Tried a basic SQL injection in the password field.**
    
    I entered the common payload:
    
    ```
    ' OR '1'='1' --
    
    ```
    
    The site responded **“sqli detected”**, which told me the app had a signature‑based filter blocking obvious SQLi patterns.
    
- **Checked the page source and found a hidden debug field.**
    
    In the HTML I found:
    
    ```html
    <input type="hidden" name="debug" value="0">
    
    ```
    
    That means every form submission included `debug=0` by default in the POST body (the page was sending that value to the server).
    
- **Changed the hidden `debug` value to `1` and submitted again.**
    
    I edited the page in my browser (DevTools) to set `debug=1` and submitted the form. Because the server code checked that POST value and printed debugging info, it returned the SQL it executed. The server-side logic is basically this:
    
    ```php
    // pseudo-PHP on the server
    if ($_POST['debug'] == '1') {
        echo "SQL query: $sql";
    }
    
    ```
    
    With `debug=1` the server echoed something like:
    
    ```
    SELECT * FROM users WHERE name='admin' AND password='1234'
    
    ```
    
    Seeing the raw SQL showed how inputs were concatenated: both `name` and `password` were placed inside single quotes.
    
- **Used the debug SQL and error messages to learn more.**
    
    Because the SQL used single quotes (`name='...' AND password='...'`), any SQLi would need to close the server's quote first. I also tested a `#` comment (MySQL style) and got a database error. The server returned:
    
    ```
    Warning: SQLite3::query(): Unable to prepare statement: 1, unrecognized token: "#" ...
    
    ```
    
    That SQLite error told me the database engine was **SQLite**, not MySQL — so `#` is not a valid comment token here.
    
- **Attacked the username field instead of the password field.**
    
    The site was detecting obvious payloads in the password field, so I tried injecting into the username to comment out the password check. I used:
    
    ```
    username: admin' --
    password: 123
    
    ```
    
    (Important: for SQLite the comment token `--` must be followed by a space, so I put a trailing space after `--`.)
    
- **Observed the final debug SQL and why it worked.**
    
    With that username the server showed:
    
    ```
    SELECT * FROM users WHERE name='admin' -- ' AND password='123'
    
    ```
    
    The `--`  begins a single‑line comment, so everything after `--`  (including the stray `' AND password='123'`) is treated as a comment and ignored by SQLite. The database therefore executed this effective query:
    
    ```
    SELECT * FROM users WHERE name='admin'
    
    ```
    
    That returned the admin row and the site logged me in.
    
- **Got the flag.**
    
    After logging in the page displayed the flag:
