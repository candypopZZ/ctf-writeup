# CTF Writeup: Flag Command

## 🧩Challenge Info
**Category: Web**

Embark on the "Dimensional Escape Quest" where you wake up in a mysterious forest maze that's not quite of this world. Navigate singing squirrels, mischievous nymphs, and grumpy wizards in a whimsical labyrinth that may lead to otherworldly surprises. Will you conquer the enchanted maze or find yourself lost in a different dimension of magical challenges? The journey unfolds in this mystical escape!

## Steps Taken

This is a usual terminal fake terminal in the browser.

Open our console and let's look at the full options list:
```bash
fetch("/api/options")
  .then(res => res.json())
  .then(data => console.log(data));
```

It gives us this:
```
allPossibleCommands
: 
1
: 
(4) ['HEAD NORTH', 'HEAD WEST', 'HEAD EAST', 'HEAD SOUTH']
2
: 
(4) ['GO DEEPER INTO THE FOREST', 'FOLLOW A MYSTERIOUS PATH', 'CLIMB A TREE', 'TURN BACK']
3
: 
(4) ['EXPLORE A CAVE', 'CROSS A RICKETY BRIDGE', 'FOLLOW A GLOWING BUTTERFLY', 'SET UP CAMP']
4
: 
(4) ['ENTER A MAGICAL PORTAL', 'SWIM ACROSS A MYSTERIOUS LAKE', 'FOLLOW A SINGING SQUIRREL', 'BUILD A RAFT AND SAIL DOWNSTREAM']
secret
: 
['Blip-blop, in a pickle with a hiccup! Shmiggity-shmack']
```

Got the secret command. Run it in your terminal.

🏆```Flag: HTB{D3v3l0p3r_t00l5_4r3_b35t_wh4t_y0u_Th1nk??!_ddb7748f8ec249b448e20d478dbb61ab}```
