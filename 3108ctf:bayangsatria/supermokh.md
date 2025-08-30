# CTF Writeup: SuperMokh

## 🧩Challenge Info
**Category: Web**

Di padang hijau berlari laju, SuperMokh gol tiada terhenti, Walau zaman sudah berlalu, Adakah anda peminat sejati?

```bash
https://supermokh.bahterasiber.my
```

## Steps Taken

Page source revealed a base-64 string and gave us a the credential to login, ```guest:Selangor1972_1987```

**Inspect cookies**

Login as guest.

Open ```DevTools → Application → Cookies.```

**Note existing cookies: ```auth_token (JWT) and PHPSESSID.```**

**Analyze token**

auth_token is a JWT:
```
{
  "username": "guest",
  "role": "visitor",
  "iat": ....,
  "exp": ....
}
```

To get the flag, I need to be SuperMokh, not guest.

Go to ```jwt.io```. From the value of the auth_token, we can manipulate the name=SuperMokh and encode it back to JWT value. Replace the original cookie with the new one. Andd got the flag!

🏆```Flag: 3108{}```
