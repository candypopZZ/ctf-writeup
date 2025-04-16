# 60 Pixels Under

**Creator:** @arifpeycal

## Description
Here’s a screenshot of a Genshin Impact redemption code . The catch? The code is there, but it appears "cropped" or incomplete. If only you could enlarge the image to reveal the rest of it.

**Hint:** *Maybe try reading up on the PNG file format.*
![codeeeee](https://github.com/candypopZZ/ctf-writeup/blob/forensics/codeeeee.JPG?raw=true)

## Solution

We were given a file named code.PNG, but the flag appeared to be cropped horizontally.

### 1. Analyze the file format

To verify whether it was a valid PNG file, I used the `exiftool` command. The output confirmed that the image dimensions were **821×517** pixels, matching the values shown in the file properties. Since our goal was to reveal the hidden part of the flag, we needed to **extend the image’s height**.

![info.JPG](https://github.com/candypopZZ/ctf-writeup/blob/forensics/info.JPG?raw=true)

### 2. Use `xxd` to view the file header

I used the `xxd` command to examine the file header. The first four bytes matched the standard PNG file signature, which I confirmed using Gary Kessler’s file signature reference. Right after that, the file displayed the **chunk labeled IHDR**. 

![000](https://github.com/candypopZZ/ctf-writeup/blob/forensics/000.JPG?raw=true)

Curious about its meaning, I Googled it and found that the IHDR chunk contains crucial **image metadata** — such as width, height, bit depth, color type, compression method, filter method, and interlace method.



This metadata is located in the chunk’s data section. In my case, the data part read 0000 0335 0000 0205, which in hexadecimal translates to 821 (width) and 517 (height) in decimal

### 4. Modify the Height
The IHDR chunk contains information about the image, including the width and height. I decoded the hex value to get the width and height, which were **821x517 pixels**. To view the cropped part of the image, I modified the height value from `517` to `450` using a hex editor.

### 5. Save and View the Image
After making the change, I saved the file. The previously hidden flag was now visible in the extended image.

## Flag
