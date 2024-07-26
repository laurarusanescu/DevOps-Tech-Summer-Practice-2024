
to exit: :q! to trash all changes
         :wq to save the changes

insert:  i - type inserted text
         a - insert after the cursor
         A - type appended text

deleting: dw - del from the cursor up to the next word type
          de - del from the cursor up to the end of the word type
          d$ - del from the cursor to the end of a line type
          dd - del a whole line type
          R - replace more than one char

2w - to repeat a motion prepend it with a number

change comand:
   opreator [number] motion
operator - is what to do (ex. d for delete)
[number] - is an optional count to repeat the motion
motion - moves over the next to operate on, such as w(word), e(end of woed), $(end of the line), ect

0 - to move to the start of the line use a zero

undo:
u - to undo previous actions
U - to undo all the changes on a line

p - to put back text that jas just been deleted
r -to replace the char under the cursor and then the char tha you want to have instead
c$ - to change untill the end of the line

change:
    c [number] monion

CTRL-G - displays your location in the file and the file status
     G - moves to the end of the file
   numn G - moves to the line number
    gg - moves to the first line

search: 
/ folowed by a phrase searches forward for the phase
? followed by a phrase searches backward for the phase 

% - while the cursor is on a (, ), {, }, [, ] goes to its match

substitute:
:s/old/new - subst new from the first old in a line type
:s/old/new/g - subst new for all 'old's on a line type
:#,#s/old/new/g - subst phases between two line #'s type
:%s/old/new/g - subst all occurences in the file type
:%s/old/new/gc - ask for confirmation each time add 'c'

:!command - executes an external command
:w FILENAME - writes the current vim file to disk with name FILENAME
v motion :w FILENAME - saves visually selected lines in file FILENAME
:r FILENAME - retrives disk file FILENAME and puts it below he cursor position
:r !dir - reads the output of the dir command and puts it below the cursor position

copy and paste:
y - copy
p - paste







