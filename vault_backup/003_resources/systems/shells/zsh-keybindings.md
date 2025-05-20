---
title: zsh-keybindings
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: command-reference
system: 
tags:
  - commands
  - uiowa
related_runbooks:
---

## Description

Here’s a simple keybinding reference for navigating the `zsh` shell on macOS. These are default shortcuts that are handy for quickly moving around the command line:

## **Cursor Movement**

- **`Ctrl + A`**: Move the cursor to the beginning of the line.
- **`Ctrl + E`**: Move the cursor to the end of the line.
- **`Ctrl + B`**: Move the cursor one character backward.
- **`Ctrl + F`**: Move the cursor one character forward.
- **`Option + B`**: Move the cursor one word backward.
- **`Option + F`**: Move the cursor one word forward.

## **Editing**

- **`Ctrl + W`**: Delete the word before the cursor.
- **`Ctrl + U`**: Delete everything before the cursor.
- **`Ctrl + K`**: Delete everything after the cursor.
- **`Ctrl + D`**: Delete the character under the cursor (or exit if the line is empty).
- **`Ctrl + H`**: Delete the character before the cursor (same as backspace).

## **Cut/Copy/Paste**

- **`Ctrl + Y`**: Paste (yank) the last cut text at the cursor position.
- **`Ctrl + K`**: Cut from the cursor to the end of the line (kills).
- **`Option + ←`**: Jump to the beginning of the previous word and select it (useful for copying text).

## **Undo/Redo**

- **`Ctrl + _`**: Undo the last change.
  
## **History Navigation**

- **`Ctrl + R`**: Search command history (incremental search).
- **`Up/Down Arrow`**: Scroll through previous commands.

## **Process Control**

- **`Ctrl + C`**: Terminate the running process.
- **`Ctrl + Z`**: Suspend the current process.
- **`fg`**: Resume the suspended process.
- **`Ctrl + L`**: Clear the terminal screen.

## **Completion**

- **`Tab`**: Auto-complete file names, directories, or commands.

## **Miscellaneous**

- **`Ctrl + G`**: Escape from history search or incomplete command.
- **`Ctrl + X Ctrl + E`**: Open the current command in your default editor (useful for multi-line commands).