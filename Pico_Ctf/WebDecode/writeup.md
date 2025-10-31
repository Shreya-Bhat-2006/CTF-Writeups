# **Challenge Name:** WebDecode




PicoCTF practice problems :

https://play.picoctf.org/practice?page=3


### **Description:**

*The challenge hints that the flag is hidden somewhere in the web page and can be found by using the web inspector.*

---

## **How I Solved the Challenge (Step-By-Step)**

1. **First, I opened the challenge webpage.**
    
    The description said: *"Do you know how to use the web inspector? Start searching here to find the flag."*
    
    So I understood that the flag is hidden inside the webpage code (HTML/CSS), not directly visible on the screen.
    
2. **I tried checking the page source of the Home page.**
    - I pressed **CTRL + U** to view the page source.
    - I searched inside the code, but I **did not find any flag**.
3. **Then, I tried using Inspect Element.**
    - I **right-clicked on the page** and selected **Inspect**.
    - I went to the **Sources** tab.
    - I saw files like:
        - `index.html`
        - `style.css`
        - some image `.gif` files
    - I checked them one by one, but still **no flag**.
4. **So instead of searching only the Home page, I moved to the About page.**
    - I clicked on **About** in the navigation.
    - Again, I **right-clicked** → **Inspect**.
5. **Inside the About page's HTML (about.html), I carefully looked through the code.**
    - Here I found something **suspicious** inside the `<section>` tag:
        
        ```
        notify_true="cGljb0NURnt3ZWJfc3VjYzNzc2Z1bGx5X2QzYzBkZWRfMWY4MzI2MTV9"
        
        ```
        
    - This looked like **Base64 encoded text**.
6. **I copied the encoded string** and decoded it using a Base64 decoder.
    
    Example decoder: https://www.base64decode.org/
    
7. **Decoded Output:**
    
    ```
    picoCTF{Flag}
    
    ```
    
8. **This is the final flag. ✅**

---

## **Final Flag**

```
picoCTF{Flag}

```

---

## **Errors / Learning Points**

| Step | Issue | How I Solved It |
| --- | --- | --- |
| Checked Home page code | Could not find flag | Realized the challenge may hide flag in other pages too |
| Searched CSS & image files | Nothing found | Focused back on `.html` files |
| Was not sure about encoded text | Recognized it as Base64 | Used online decoder to decode it |

---

## **What I Learned**

- How to use **Inspect Element (F12 or Right-click → Inspect)**.
- How to explore **Sources** panel to view webpage files.
- Recognizing **Base64 encoded strings**.
- How to decode Base64 to reveal hidden data.






⚠️ Safety note 
-
I confirm that I have NOT published any full flags, personal data, credentials, or harmful exploit instructions in this repository. All writeups show proof of solving with flags redacted (e.g. FLAG{REDACTED}) and are shared only for learning and discussion.

