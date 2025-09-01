# CTF Writeup: Dynastic

## 🧩Challenge Info
**Category: Cryptography**

You find yourself trapped inside a sealed gas chamber, and suddenly, the air is pierced by the sound of a distorted voice played through a pre-recorded tape. Through this eerie transmission, you discover that within the next 15 minutes, this very chamber will be inundated with lethal hydrogen cyanide. As the tape’s message concludes, a sudden mechanical whirring fills the chamber, followed by the ominous ticking of a clock. You realise that each beat is one step closer to death. Darkness envelops you, your right hand restrained by handcuffs, and the exit door is locked. Your situation deteriorates as you realise that both the door and the handcuffs demand the same passcode to unlock. Panic is a luxury you cannot afford; swift action is imperative. As you explore your surroundings, your trembling fingers encounter a torch. Instantly, upon flipping the switch, the chamber is bathed in a dim glow, unveiling cryptic letters etched into the walls and a disturbing image of a Roman emperor drawn in blood. Decrypting the letters will provide you the key required to unlock the locks. Use the torch wisely as its battery is almost drained out!

## Steps Taken

```
┌──(kali㉿kali)-[~/htbtryout/dynastic/crypto_dynastic] └─$ ls output.txt source.py ┌──(kali㉿kali)-[~/htbtryout/dynastic/crypto_dynastic] └─$ cat output.txt Make sure you wrap the decrypted text with the HTB flag format :-] DJF_CTA_SWYH_NPDKK_MBZ_QPHTIGPMZY_KRZSQE?!_ZL_CN_PGLIMCU_YU_KJODME_RYGZXL ┌──(kali㉿kali)-[~/htbtryout/dynastic/crypto_dynastic] └─$ cat source.py from secret import FLAG from random import randint def to_identity_map(a): return ord(a) - 0x41 def from_identity_map(a): return chr(a % 26 + 0x41) def encrypt(m): c = '' for i in range(len(m)): ch = m[i] if not ch.isalpha(): ech = ch else: chi = to_identity_map(ch) ech = from_identity_map(chi + i) c += ech return c with open('output.txt', 'w') as f: f.write('Make sure you wrap the decrypted text with the HTB flag format :-]\n') f.write(encrypt(FLAG))
```

How the encryption works (encrypt(m)):

For each character in the plaintext m:

If it’s not a letter, it’s copied unchanged.

If it is a letter:

Convert to number: ```ord(ch) - 0x41 → A=0, B=1, ..., Z=25.```

Add the position index i.

Convert back to letter with ```modulo 26.```

So basically:
```
ciphertext[i] = (plaintext[i] + i) mod 26
```

How to decrypt it?

Since:
```
cipher[i] = (plain[i] + i) mod 26
```

We invert it:
```
plain[i] = (cipher[i] - i) mod 26
```

Non-alphabet characters (like _ or ?!) are kept as-is.

Run this Python script:

```bash
def to_identity_map(a):
    return ord(a) - 0x41

def from_identity_map(a):
    return chr(a % 26 + 0x41)

# read ciphertext (2nd line of output.txt)
with open("output.txt", "r") as f:
    f.readline()  # skip the first line
    ciphertext = f.readline().strip()

plaintext = ""
for i, ch in enumerate(ciphertext):
    if not ch.isalpha():
        plaintext += ch
    else:
        chi = to_identity_map(ch)
        # reverse encryption: subtract index
        p = (chi - i) % 26
        plaintext += from_identity_map(p)

print("HTB{" + plaintext + "}")
```

🏆```Flag: HTB{DID_YOU_KNOW_ABOUT_THE_TRITHEMIUS_CIPHER?!_IT_IS_SIMILAR_TO_CAESAR_CIPHER}```
