---
title: py-bulk-renaming-script
created: 2024-10-30
modified: 2024-10-30
type: resource
category:
  - tool|system|procedure
tags:
  - resource
  - python
  - development
related_docs:
---
## Description
This guide details the process of running a Python script designed to rename files and folders by converting all uppercase letters to lowercase and replacing spaces with hyphens. Additionally, it ensures that any occurrences of " - " (space, hyphen, space) are replaced with a single hyphen "-". This renaming practice can help in standardizing file and directory names for better organization and easier access, particularly useful in environments where file naming conventions are crucial.

## Prerequisites
- **Python Installed**: Ensure that Python 3 is installed on your computer. You can download it from [python.org](https://www.python.org/downloads/).
- **Script File**: You need the script file saved on your computer. You can copy the code provided below into a text editor and save it as `rename_files_folders.py`.
- **File Backup**: Before running the script, ensure that you have backups of your files and folders. This script modifies file and directory names, which is irreversible.

## Step-by-Step Procedure
1. **Prepare Your Script**:
   - Open a text editor (like Notepad or VSCode).
   - Copy the following script and paste it into the editor.

```python
import os
from pathlib import Path

def rename_files_folders(start_path):
    for root, dirs, files in os.walk(start_path, topdown=False):
        for name in files:
            old_file_path = Path(root) / name
            new_file_name = name.lower().replace(" ", "-").replace("---", "-").replace("--", "-")
            new_file_path = Path(root) / new_file_name
            os.rename(old_file_path, new_file_path)
            print(f"Renamed file: {old_file_path} to {new_file_path}")

        for name in dirs:
            old_dir_path = Path(root) / name
            new_dir_name = name.lower().replace(" ", "-").replace("---", "-").replace("--", "-")
            new_dir_path = Path(root) / new_dir_name
            os.rename(old_dir_path, new_dir_path)
            print(f"Renamed directory: {old_dir_path} to {new_dir_path}")

if __name__ == "__main__":
    start_path = input("Enter the path of the folder you want to modify: ").strip()
    rename_files_folders(start_path)
    print("Renaming completed successfully.")
```
   - Save the file with a `.py` extension, for example, `rename_files_folders.py`.

2. **Run the Script**:
   - Open your command line interface (CLI) such as Command Prompt on Windows or Terminal on macOS and Linux.
   - Navigate to the directory where your script is saved. For example, if it is saved in `C:\scripts`, you would type `cd C:\scripts`.
   - Run the script by typing `python rename_files_folders.py` and press Enter.
   - When prompted, enter the absolute path of the folder you wish to modify and press Enter.

3. **Verify the Changes**:
   - Go to the folder you specified and verify that the file and folder names have been renamed as expected.
   - If there are subdirectories, check them as well to ensure all names have been updated.

## Troubleshooting
- **Permission Issues**: If the script fails to rename some files or folders, check if you have proper permissions to modify these files. Running the command line as an administrator or using `sudo` on Linux/macOS might resolve this issue.
- **Python Not Recognized**: If the command line does not recognize the `python` command, ensure Python is correctly installed and its path is added to your system's environment variables.
- **Script Does Not Run**: Check for typos in the script or command line. Ensure the file path you provide is correct and accessible.

This guide should help you effectively use the renaming script to organize your files and directories more efficiently. Remember, always test scripts like this in a controlled environment (such as on a small set of sample files) before running them on a large scale.