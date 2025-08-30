# CTF Writeup: Permainan Lagenda

## 🧩Challenge Info
**Category: Miscellaneous**

Di sinilah bermulanya era permainan video sebelum wujudnya MLBB, PUBG, CSGO dan lain-lain. Bantu Soloz dalam permainan ini untuk mendapatkan flag.

```bash
https://ular.bahterasiber.my
```

## Steps Taken

I’m not good at PC gaming, so I wasn’t sure if I could reach 400 points to get the flag. How about another way?

Look at this part near the end of ```game.js```:
```bash
window['game'] = {
  'getScore': () => score,
  'setScore': _0x4c9492 => { score = Number(_0x4c9492) || 0; updateScore(); return score; },
  'addPoints': _0x193747 => { score += Number(_0x193747) || 0; updateScore(); return score; },
  'triggerFlag': () => { revealFlag(); },
  'reset': () => { reset(true); }
};
```

Basically, the game exposes a game object on window with some functions:

```game.setScore(400)``` → sets score to 400 directly and updates the game state.

```game.addPoints(400)``` → adds 400 points to our current score.

```game.triggerFlag()``` → immediately shows the flag without changing our score.


**Open F12 (Web Developer Tools) and console**
```bash
game.setScore(400);
```
Got the flag even without playing!

🏆```Flag: 3108{}```
