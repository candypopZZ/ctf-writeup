# 📁 File types 

**Author:** Geoffrey Njogu  
**Category:** Forensics  
**Level:** Medium  
**Tags:** `forensics`, `file analysis`, `compression`, `picoctf 2022`

---

## 🧠 Challenge Description

> This file was found among some files marked confidential but my pdf reader cannot read it, maybe yours can.  
> _You can download the file from here: `Flag.pdf`._

---

## 🧩 Walkthrough

### 🔍 Step 1: Inspect the file

```bash
file Flag.pdf
exiftool Flag.pdf
```

📌 It’s not really a PDF. It’s actually a shell archive ```(shar)```, a script that extracts files when run.

### 📦 Step 2: Run the shell archive

```bash
sh Flag.pdf
```

This script extracted a file named **flag**.

### 📂 Step 3: Check and extract the archive
```bash
file flag
cpio -id < flag
```

➡️ Extracted final, which looked like a **compressed binary blob**.

### 🧪 Step 4: Identify the compression layers
```bash
file final
binwalk final
```

Multiple layers were detected:
**LZ4 → LZMA → LZOP → LZIP → XZ**

### 🗜️ Step 5: Decompress recursively
```bash
lz4 -d final -o final.lzma
lzma -d final.lzma           # becomes final (LZOP)
lzop -d final                # becomes final_decompressed
lzip -d final_decompressed   # becomes final_decompressed.out
mv final_decompressed.out final.xz
xz -d final.xz               # becomes final (ASCII hex string)
```

### 🔓 Step 6: Read the final result
```bash
cat final
```

Found hex string:

```7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f6630725f3062326375723137795f33633739633562617d0a```

Convert it:

```bash
echo 7069636f4354467b66316c656e406d335f6d406e3170756c407431306e5f6630725f3062326375723137795f33633739633562617d0a | xxd -r -p
```

🏁 Flag captured!

```picoCTF{f1len@m3_m@n1pul@t10n_f0r_0b2cur17y_3c79c5ba}```
