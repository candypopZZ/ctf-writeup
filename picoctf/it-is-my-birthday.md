# 📌 It is my Birthday

## 📌 Challenge Info
- **Author**: madStacks  
- **Category**: Web Exploitation  
- **Level**: Medium  
- **Tags**: picoCTF2021, md5, hash collision, php, file upload

## 🧩 Description
> I sent out 2 invitations to all of my friends for my birthday!  
> I'll know if they get stolen because the two invites look similar, and they even have the same MD5 hash, but they are slightly different!  
> You wouldn't believe how long it took me to find a collision. Anyway, see if you're invited by submitting 2 PDFs to my website.

---

## 💡 Analysis

From the description, we inferred that the backend likely performs a PHP check similar to:

```php
if (md5(file1) == md5(file2) && file1 != file2)
```
Thus, to pass the check:
**Both files** must have the **same MD5 hash.**
The **actual binary contents must differ** (so != in PHP is true).

---

## What to Do? Use Pre-Generated Colliding PDFs

Instead of generating from scratch, we used pre-made PDF collision examples from:

🔗 [corkami/collisions – examples/free](https://github.com/corkami/collisions/blob/master/examples/free/README.md)

#### Files Used:
- [`md5-1.pdf`](https://github.com/corkami/collisions/blob/master/examples/free/md5-1.pdf)
- [`md5-2.pdf`](https://github.com/corkami/collisions/blob/master/examples/free/md5-2.pdf)

Submit both files and just like that, you got the flag!

![flag]()
