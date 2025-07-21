# 🔐: Some Assembly Required 1

- **Author:** Sears Schulz  
- **Category:** Web Exploitation  
- **Difficulty:** Medium  
- **Tags:** `picoctf 2021`, `web`, `wasm`, `javascript`, `reversing`  
- **Challenge URL:** [http://mercury.picoctf.net:40226/index.html](http://mercury.picoctf.net:40226/index.html)

---

## 🔍 Summary

The challenge loads a WebAssembly module `JIFxzHyW8W` and verifies input via two functions:
- `copy_char` – copies user input into memory
- `check_flag` – checks if the input is correct

This implies the flag verification logic is inside the `.wasm` file.

---

## 🛠️ Walkthrough

1. **Page blocked direct access** to `JIFxzHyW8W`, returning:
   > *"Please refresh to debug this module"*

2. **Used DevTools Console** to download the `.wasm` file:
   ```js
   (async () => {
     const res = await fetch('./JIFxzHyW8W');
     const buf = await res.arrayBuffer();
     const blob = new Blob([buf], { type: 'application/wasm' });
     const url = URL.createObjectURL(blob);
     console.log('Download WASM:', url);
   })();
   ```
   
3. **Opened the file** using:
    ```bash
    cat JIFxzHyW8W.wasm
    ```

4. **Found the flag** near the end of the binary content:
   ```picoCTF{cb688c00b5a2ede7eaedcae883735759}```

---

## ✅ Lessons Learned

- How WebAssembly modules interact with JavaScript

- Fetching and analyzing WASM files using DevTools

- The importance of checking binary resources for hardcoded secrets

---

## 🏁 STATUS: SOLVED!
