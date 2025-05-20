---
type: reference
refrenceName: less-command
tags:
  - unix/commands
  - terminal
source: "{{source}}"
date: 2024-05-29
---

### Searching with the `less` Command

#### Opening a File
- **Command:** `less filename`
  - Replace `filename` with the actual file name.

#### Initiating a Search
- **Forward Search:**
  - Press `/`
  - Type the text you want to search for and press Enter.
- **Backward Search:**
  - Press `?`
  - Type the text you want to search for and press Enter.

#### Navigating Through Matches
- **Next Match:** Press `n`
- **Previous Match:** Press `N` (Shift + n)

#### Case Sensitivity
- **Toggle Case Sensitivity:** Type `-i` before starting your search with `/`.

#### Exiting `less`
- **Exit Command:** Press `q`

### Additional Features and Commands in `less`

#### Navigating Through the File
- **Move Down One Line:** Press `j` or the Down Arrow key.
- **Move Up One Line:** Press `k` or the Up Arrow key.
- **Move Down One Page:** Press `Space` or `Ctrl+F`.
- **Move Up One Page:** Press `b` or `Ctrl+B`.
- **Go to Beginning of File:** Press `g`.
- **Go to End of File:** Press `G`.

#### Displaying Line Numbers
- **Toggle Line Numbers:** Press `-N`.

#### Marking and Jumping to Positions
- **Set a Mark:** Press `m` followed by any lowercase letter (e.g., `ma`).
- **Return to Mark:** Press `'` followed by the corresponding letter (e.g., `'a`).

#### Viewing Multiple Files
- **Open Multiple Files:** `less file1 file2 ...`
- **Next File:** Press `:n`.
- **Previous File:** Press `:p`.

#### Changing Display Options
- **Toggle Case Sensitivity for Searches:** Type `-i`.
- **Wrap Long Lines:** Type `-S` to toggle line wrapping on and off.

#### Saving Changes
- **Save Current Content to File:** Press `s` and then type the filename to save the current buffer to a file.

#### Miscellaneous
- **Show Help:** Press `h` for a summary of all commands.
- **Refresh Display:** Press `Ctrl+R`.
