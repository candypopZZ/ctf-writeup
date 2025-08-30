# CTF Writeup: Operation Nyet

## 🧩Challenge Info
**Category: Forensics**

Pada suatu hari, ketika Khairul Aming meninggalkan laptopnya tanpa pengawasan, seorang staf menyambungkan USB miliknya ke laptop tersebut dan melakukan sesuatu.

Beberapa saat kemudian, dia mencabut USB itu dan beredar. Tindakannya tidak disedari Khairul Aming, namun sempat diperhatikan oleh seorang rakan sekerja yang berasa curiga.

Beberapa jam kemudian, USB tersebut secara cuai ditinggalkan di atas mejanya. Rakan sekerja itu mengambil USB tersebut kerana ingin mengetahui rahsia di dalamnya.

Kini, tugas anda adalah untuk menyiasat isi kandungan USB tersebut melalui fail imej forensik yang diberikan (.E01).

## Steps Taken

Run:
```bash
file USB.E01
```

Result:
```
EWF/Expert Witness/EnCase image file format
```

Confirmed it is an EnCase image.

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

The raw image ewf1 contains the USB filesystem. Mount it read-only:
```bash
sudo mkdir /mnt/usb_raw
sudo mount -o ro,loop /mnt/usb_img/ewf1 /mnt/usb_raw
```

Explore files:
```bash
cd /mnt/usb_raw
sudo ls -la
```

Found bunch of files. From USBBackup___.bat, combine the Base64 segments:
```
tmp1 = !xA!!q7!       → MzE + wOH
tmp2 = !jK!!X4!       → tue + WV0
tmp3 = !kQ!!Y9!       → X25 + 5ZX
tmp4 = !zn!!P2!       → Rfcm + Foc2
tmp5 = !LM!!vZ!       → lhX2 + 55ZX
tmp6 = !aX!!d3!!uT!   → Rfbn + lldH + 0=
```

to be...```MzEwOHtueWV0X255ZXRfcmFoc2lhX255ZXRfbnlldH0```

Use cyberchef or any online decoder.

🏆```Flag: 3108{nyet_nyet_rahsia_nyet_nyet}```
