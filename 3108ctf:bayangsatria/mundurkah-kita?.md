# CTF Writeup: Mundurkah kita?
 
## 🧩Challenge Info
**Category: Reversing**

Apakah rahsia yang tersembunyi di alam sebalik mata ini.

**File:** simple_calculator.zip

**MD5:** a4ed109e7a58b2538fee130127352c22

**SHA1:** 8de104194af3cb1899631735503fe2d8b69b6fde

## Steps Taken

**Verify Binary**
```bash
md5sum simple_calculator.exe
sha1sum simple_calculator.exe
```

It reveals different hashes from what provided in the description, meaning the file has been modified.

**Check Strings**
```bash
strings simple_calculator.exe | grep 3108
```

🏆```Flag: 3108{nothing_beats_the_string_method}```
