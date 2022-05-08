- (.): repeat last command
- Esc: escape to command mode
- u: undo
- Ctrl+R: redo
- Caps Lock: beware of capslock
- (:!command): run a (shell)command, enter to get back
- Ctrl+g: show file information
### Search
- /text: search forward
- ?text: search backward
n = search next same direction
N = opposite direction

search can also use wildcards such as *, ?, [], {}

### Paste
- P: Puts the copied text to the left of the cursor if the text consists of letters or words; puts the copied text above the current line if the copied text contains lines of text. 
- p: Puts the buffered text to the right of the cursor 

### Exit
- ZZ: Saves the current changes to the file and exits from vi. 
- (:w): Saves the current file, but you can continue editing. 
- (:wq): Works the same as ZZ. 
- (:q): Quits the current file. This works only if you don’t have any unsaved changes. 
- (:q!): Quits the current file and doesn’t save the changes you just made to the file