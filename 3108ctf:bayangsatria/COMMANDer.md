# CTF Writeup: COMMANDer

## 🧩Challenge Info
**Category: Web**

Terminal lama ini menyimpan biodata seseorang bersama rahsianya. Namun, rahsia itu hanya akan terbuka kepada mereka yang tahu menggunakan arahan yang tepat. Mampukah anda menguasai terminal ini untuk membongkar kebenaran?

```bash
https://komander.bahterasiber.my
```

## Steps Taken

Important observation from ```main.js```:

```bash
if (availableOptions["secret"].includes(currentCommand)) { ... }
```

This indicates a secret command exists that gives the flag.

**Open F12 and go to your console**
```bash
fetch('/api/pilihan')
  .then(r => r.json())
  .then(data => console.log(data.allPossibleCommands));
```

Output:
```bash
{
  "1": ["22 Januari 1911", "19 Oktober 1922", "12 Disember 1920", "15 Februari 1913"],
  "2": ["Kuala Lumpur", "Pulau Pinang", "Johor Bahru", "Ipoh"],
  "3": ["Ketua Turus Tentera Darat", "Panglima Medan", "Ketua Turus Angkatan Tentera Malaysia", "Penasihat Pertahanan"],
  "4": ["1 Ogos 1965", "31 Ogos 1957", "1 Julai 1970", "16 September 1971"],
  "secret": ["RAHSIA: OperationOatmeal"]
}
```

Let's call API directly from the console:
```bash
fetch('/api/check', {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ command: "RAHSIA: OperationOatmeal", step: 1 })
})
.then(r => r.json())
.then(console.log);
```

Response:
```bash
{
    "message": "3108{0p3R4T10n_O@Tm34l_1bR4h1M_1sM@1L}",
    "type": "flag"
}
```

🏆```Flag: 3108{0p3R4T10n_O@Tm34l_1bR4h1M_1sM@1L}```
