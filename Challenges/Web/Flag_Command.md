# Flag Command

- **Category:** Web
- **Points:** 0
- **Difficulty:** Very Easy

[Flag Command](https://app.hackthebox.com/challenges/Flag%2520Command)

## Description

Embark on the "Dimensional Escape Quest" where you wake up in a mysterious forest maze that's not quite of this world. Navigate singing squirrels, mischievous nymphs, and grumpy wizards in a whimsical labyrinth that may lead to otherworldly surprises. Will you conquer the enchanted maze or find yourself lost in a different dimension of magical challenges? The journey unfolds in this mystical escape!

## Solution

The challenge is a text-based game where you must escape a maze.
Thing is, you can't actually escape the maze, so you have to find another way to get the flag.

> If you use your browser's developer tools, you can see that the flag is in the source code of the page.

```javascript
{allPossibleCommands: {1: ["HEAD NORTH", "HEAD WEST", "HEAD EAST", "HEAD SOUTH"],…}}
secret:["Blip-blop, in a pickle with a hiccup! Shmiggity-shmack"]
```
> Just input the secret command to get the flag.

## Flag

`HTB{D3v3l0p3r_t00l5_4r3_b35t__t0015_wh4t_d0_y0u_Th1nk??}`
