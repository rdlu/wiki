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
