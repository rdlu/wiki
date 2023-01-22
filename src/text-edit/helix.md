# Helix Editor

## Selection

__`:tutor` Chapter 3 Recap__


 * Type w to select forward until the next word.
   * Type e to select to the end of the current word.
   * Type b to select backward to the start of the current word.
   * Use uppercase counterparts, W,E,B, to traverse WORDS.

 * Type d to delete the entire selection.
   * Type c to delete the selection and enter Insert mode.

 * Type a number before a motion to repeat it that many times.

 * Type v to enter Select mode, where all motions extend the
   selection.

 * Type x to select the entire current line. Type x again to
   select the next line.

 * Type semicolon ( ; ) to collapse selection.

## Undo / Copy / Search

__`:tutor` Chapter 4 Recap__

 * Type u to undo. Type U to redo.

 * Type y to yank (copy) text and p to paste.
   * Use Space + y and Space + p to yank / paste on the system
     clipboard.

 * Type / to search forward in file, and ? to search backwards.
   * Use n and N to cycle through search matches.

## Multi Cursor

__`:tutor` Chapter 5 Recap__

 * Type C to duplicate the cursor to the next suitable line
   and Alt-C for previous suitable line.

 * Type s to select all instances of a regex pattern inside
   the current selection.

 * Type & to align selections.

 * Type Alt-s to split the selection into lines.

## Selection with Search / Replace

__`:tutor` Chapter 6 Recap__

 * Type f / F to extend selection up to & including a character.
   * Type t / T to extend selection until a character.

 * Type r to replace selected characters.

 * Type . to repeat the last insertion.
   * Type Alt-. to repeat the last f / t selection.

## Advanced Manipulation

__`:tutor` Chapter 7 Recap__


 * Type R to replace the selection with yanked text.

 * Type J to join lines in selection.

 * Type < and > to indent / outdent lines.

 * Type Ctrl-a to increment the selected number.
   * Type Ctrl-x to decrement the selected number.

## Search

__`:tutor` Chapter 9 Recap__


 * Type * to set the search register to the primary selection.

 * Type n / N in Visual mode to add selections on each search
   match.

 * Type Ctrl-s to save position to the jumplist.
   * Type Ctrl-i and Ctrl-o to go forward and backward in the
     jumplist.

# Manipulating Selections


__`:tutor` Chapter 10 Recap__
 * Use ) and ( to cycle the primary selection back and forward
   through selections respectively.
   * Type Alt-, to remove the primary selection.

 * Type ~ to alternate case of selected letters.
   * Use ` and Alt-` to set the case of selected letters to
     upper and lower respectively.

 * Type S to split selections on regex.

## Troubleshooting Gnome Alt+` backtick

You can set only Super+` for App switch.

     gsettings set org.gnome.desktop.wm.keybindings switch-group "['<Super>Above_Tab']"
