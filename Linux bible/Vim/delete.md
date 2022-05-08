- x: Deletes the character under the cursor. 
- X: Deletes the character directly before the cursor. 
- d(): Deletes some text. 
- c(): Changes some text. 
- y(): Yanks (copies) some text.

substitue () with movement key, example:
- w: word after cursor
- b: word before cursor
- same character: entire line
- $: from cursor to end of line
- 0: from cursor to beginning of line
- l: current letter(right)
- ): current sentences
- }: current paragraph

can also be modified with number, basically repeat the command example:
- 3dd: Deletes (d) three (3) lines (d), beginning at the current line. 
- 3dw: Deletes (d) the next three (3) words (w). 
- 5cl: Changes (c) the next five (5) letters (l) (that is, removes the letters and enters input mode). 
- 12j: Moves down (j) 12 lines (12). 
- 5cw: Erases (c) the next five (5) words (w) and goes into input mode. 
- 4y): Copies (y) the next four (4) sentences ( ) )