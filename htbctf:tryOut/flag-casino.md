# CTF Writeup: Flag Casino

## 🧩Challenge Info
**Category: Reversing**

The team stumbles into a long-abandoned casino. As you enter, the lights and music whir to life, and a staff of robots begin moving around and offering games, while skeletons of prewar patrons are slumped at slot machines. A robotic dealer waves you over and promises great wealth if you can win - can you beat the house and gather funds for the mission?

## Steps Taken

We got an ELF binary file. I skim through and found readable string:
```
WELCOME TO ROBO CASINO
PLEASE PLACE YOUR BETS
CORRECT
INCORRECT
ACTIVATING SECURITY SYSTEM
```

So this challenge is a normal reversing challenge. Might consist of characters input, programmed using ```srand/rand``` or compare with array check.

Run this script in your VM to dump array check[] directly from binary and reconstruct flag.


solve_casino.py:
```bash
from pwn import *
import ctypes

# Load binary
elf = ELF("./casino")

# Get address of check[] array (symbol ada dalam binary)
check_addr = elf.symbols['check']

# There are 29 integers (based on reversing)
n = 29
raw = elf.read(check_addr, n * 4)

# Convert to list of ints
check = [u32(raw[i:i+4]) for i in range(0, len(raw), 4)]
print("[+] Extracted check array:", check)

# Setup libc rand
libc = ctypes.CDLL("libc.so.6")

flag = ""
for target in check:
    found = None
    for c in range(256):  # try all possible chars
        libc.srand(c)
        if libc.rand() == target:
            found = chr(c)
            break
    if found:
        flag += found
    else:
        flag += "?"  # just in case
print("[+] Flag =", flag)
```

How to run:
```bash
python3 solve_flagcasino.py
```

Voila!
```
┌──(kali㉿kali)-[~/htbtryout/flagcasino/rev_flagcasino]
└─$ python3 solve_casino.py
[*] '/home/kali/htbtryout/flagcasino/rev_flagcasino/casino'
    Arch:       amd64-64-little
    RELRO:      Partial RELRO
    Stack:      No canary found
    NX:         NX enabled
    PIE:        PIE enabled
    Stripped:   No
[+] Extracted check array: [608905406, 183990277, 286129175, 128959393, 1795081523, 1322670498, 868603056, 677741240, 1127757600, 89789692, 421093279, 1127757600, 1662292864, 1633333913, 1795081523, 1819267000, 1127757600, 255697463, 1795081523, 1633333913, 677741240, 89789692, 988039572, 114810857, 1322670498, 214780621, 1473834340, 1633333913, 585743402]
[+] Flag = HTB{r4nd_1s_v3ry_pr3d1ct4bl3}
  ```   
