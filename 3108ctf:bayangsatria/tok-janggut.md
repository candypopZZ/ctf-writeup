# CTF Writeup: Tok Janggut

## 🧩Challenge Info
**Category: Forensics**

Pada tahun 1915, Tok Janggut bangkit menentang penjajahan British di Kelantan. Selepas pertempuran tragis di Pasir Puteh, satu-satunya gambar terakhir beliau disimpan dalam bentuk digital oleh seorang sejarawan moden.

Namun, gambar bersejarah ini telah diubah oleh pihak tidak bertanggungjawab, dipercayai untuk memadam bukti perjuangan beliau.

Sebagai penyiasat forensik, tugas anda adalah untuk membaik pulih fail ini dan mengesan mesej rahsia yang tersembunyi dalam gambar tersebut.

## Steps Taken

We were given Tok_Janggut.bin. File type is data. So this is not a direct image forensics.

**Hex Inspection**

Open with xxd:
```bash
xxd Tok_Janggut.bin | head
```

I noticed the first few bytes don’t match any known file header. However, scrolling further down, I spotted the familiar JPEG magic bytes:
```
00000008: ff d8 ff e0 ...
```
**FF D8 FF E0** means the start of a JPEG file. But it doesn’t start at offset 0, it starts at offset 8. Interesting.

**Carving the File**

Since the real JPEG header starts at byte 8, I need to carve out the actual image. Use dd:
```bash
dd if=Tok_Janggut.bin of=Tok_Janggut_real.jpg bs=1 skip=8
```

```if``` → input file

```of``` → output file

```bs=1``` → read 1 byte at a time

```skip=8``` → skip the first 8 bytes (the junk before real JPEG header)

Open the new file and the flag is written there.

🏆```Flag: 3108{}```
