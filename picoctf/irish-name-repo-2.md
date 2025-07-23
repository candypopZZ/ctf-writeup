# 📝 Irish-Name-Repo 2

- **Author:** Xingyang Pan 
- **Category:** Web Exploitation  
- **Difficulty:** Medium  
- **Tags:** picoCTF 2019
  
---

## 🕵️‍♀️ Summary

This is a sequel to the previous SQL injection challenge. The site’s login functionality has been "strengthened" — basic SQLi payloads like `' OR '1'='1` are now filtered.

---

## 💥 Exploit

After testing a few common payloads, we discovered that:

- Filtering is in place on the **password field**.
- **Username field** is still injectable.

### ✅ Working Payload:
- **Username:** `admin' --`
- **Password:** *(anything)*

This injects a SQL comment (`--`) to ignore the rest of the query:
```sql
SELECT * FROM users WHERE username = 'admin' -- ' AND password = '...';
```

This bypasses the password check entirely.

🎯 **We logged in as the admin and retrieved the flag. 🏁**

![irish2](https://github.com/candypopZZ/ctf-writeup/blob/main/images/irish2.JPG?raw=true)
