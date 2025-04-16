# 60 Pixels Under

**Creator:** @arifpeycal

## Description
Here’s a screenshot of a Genshin Impact redemption code . The catch? The code is there, but it appears "cropped" or incomplete. If only you could enlarge the image to reveal the rest of it.

**Hint:** *Maybe try reading up on the PNG file format.*

## Solution
![codeeeee](https://github.com/candypopZZ/ctf-writeup/blob/forensics/codeeeee.JPG?raw=true)

### 1. Analyze the File Format
I used **exiftool** to confirm the file was a PNG image.
<pre>exiftool code.PNG</pre>



The properties showed the image size as **821x517 pixels**.

### 3. Use `xxd` to View the File Header
I used the `xxd` command to analyze the hex header of the image and identify the **IHDR chunk**.

### 4. Modify the Height
The IHDR chunk contains information about the image, including the width and height. I decoded the hex value to get the width and height, which were **821x517 pixels**. To view the cropped part of the image, I modified the height value from `517` to `450` using a hex editor.

### 5. Save and View the Image
After making the change, I saved the file. The previously hidden flag was now visible in the extended image.

## Flag
