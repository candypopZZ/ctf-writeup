# CTF Writeup: Operation Nyet

## 🧩Challenge Info
**Category: Forensics**

Pada suatu hari, ketika Khairul Aming meninggalkan laptopnya tanpa pengawasan, seorang staf menyambungkan USB miliknya ke laptop tersebut dan melakukan sesuatu.

Beberapa saat kemudian, dia mencabut USB itu dan beredar. Tindakannya tidak disedari Khairul Aming, namun sempat diperhatikan oleh seorang rakan sekerja yang berasa curiga.

Beberapa jam kemudian, USB tersebut secara cuai ditinggalkan di atas mejanya. Rakan sekerja itu mengambil USB tersebut kerana ingin mengetahui rahsia di dalamnya.

Kini, tugas anda adalah untuk menyiasat isi kandungan USB tersebut melalui fail imej forensik yang diberikan (.E01).

## Steps Taken

Since the challenge description mentioned a USB device and suspicious activity, I suspected the attacker may have left behind exfiltrated data. So my first step was to confirm the image type and mount it properly.

Run:
```bash
file USB.E01
```

Result:
```
EWF/Expert Witness/EnCase image file format
```

This confirmed it is an EnCase image. I tried open the .E01 using **autopsy** but it crashed because of corrupted index. Then, I try switched to **ewfmount** and it works (because it is EnCase images haha).

**Mount the E01 image**
```bash
sudo mkdir /mnt/usb_img
sudo ewfmount USB.E01 /mnt/usb_img
```

Check contents:
```bash
sudo ls /mnt/usb_img
> ewf1
```

**Mount the raw image**

The raw image ewf1 contains the USB filesystem. Mount it read-only to avoid modifying the evidence:
```bash
sudo mkdir /mnt/usb_raw
sudo mount -o ro,loop /mnt/usb_img/ewf1 /mnt/usb_raw
```

Explore files:
```bash
cd /mnt/usb_raw
sudo ls -la
```

Found bunch of files. But I try to also checked for any deleted files using **fls** and **icat** just in case, but nothing useful appeared (just wasting my time lol).

Tried to find a way to cutdown the process of searching the correct file. Thats when I encountered the BAT file (I remembered this term/file from mobile security workshop I attended, as this relates to memory).

The directory showed several odd file names with double exclamation marks (!!). This potentially looks like obfuscation or encoding things. And yes, found something in base64 chunks.

p/s: I initially thought the strange strings were XOR encoded (haha), so I attempted XOR decoding with common keys but got no logical output. After a break (crashout moment), I noticed the pattern represent broken Base64 segments :'(.

Each chunk contained typical base64 valid characters (A–Z, a–z, 0–9, +, /, =). The final chunk also ended with ‘=’ (why i didnt noticed earlier). So this confirmed it was a base64 payload.

From USBBackup___.bat, combine the Base64 segments:
```
tmp1 = !xA!!q7!       → MzE + wOH
tmp2 = !jK!!X4!       → tue + WV0
tmp3 = !kQ!!Y9!       → X25 + 5ZX
tmp4 = !zn!!P2!       → Rfcm + Foc2
tmp5 = !LM!!vZ!       → lhX2 + 55ZX
tmp6 = !aX!!d3!!uT!   → Rfbn + lldH + 0=
```

to be...```MzEwOHtueWV0X255ZXRfcmFoc2lhX255ZXRfbnlldH0```

Interesting strings.

Use cyberchef or any online decoder.

🏆```Flag: 3108{nyet_nyet_rahsia_nyet_nyet}```

## Final Thoughts

This challenge taught me that even a simple batch file can hide multiple layers of code obfuscation. It also shows the importance of not jumping to conclusions too early and consistently validating all assumptions you ever think of and not leave any.
