---
type: reference
refrenceName: paths-in-unix-systems
tags: 
source: "{{source}}"
date: 2024-04-17
---

### What Does "Include These Directories in Your PATH" Mean?

When someone says "include these directories in your PATH," they're talking about adding specific directories to the list of paths that your operating system searches through when you run a command in the terminal or command line.

The `PATH` environment variable is a list of directories separated by colons (on Unix-like systems such as Linux and macOS) or semicolons (on Windows) where executable files are located. When you type a command, the system looks through these directories in order, trying to find the executable file that matches the command you entered.

### Why Is It Important?

Including directories in your PATH makes running scripts and programs much more convenient because you don't need to specify the full path to the executable file every time you want to run it. You can just type the name of the program or script, and if it's in one of the directories listed in your PATH, the system will find and execute it.

### How to Include Directories in Your PATH

Let's go through the steps on a Unix-like system, such as Linux or macOS:

1. **Find Out Your Current PATH**: Open a terminal and type `echo $PATH`. This command will display your current PATH variable.

2. **Decide Where to Add Your Directory**: You might add your directory at the beginning or the end of the PATH, depending on its priority. Directories at the beginning of the PATH are searched first.

3. **Add the Directory to Your PATH**:
   - **Temporarily (current session only)**: Use the command `export PATH=/path/to/your/directory:$PATH` to add it at the beginning or `export PATH=$PATH:/path/to/your/directory` to add it at the end. This change lasts until you close the terminal.
   - **Permanently**: To make the change permanent, you need to add the export command to a shell initialization file like `~/.bash_profile`, `~/.bashrc`, or `~/.zshrc`, depending on your shell. Then, every time a terminal is opened, your PATH will include the directory.

### Example

If you have scripts in `~/my-scripts` and you want to run them from anywhere, you could add this directory to your PATH. Here's how you might add it permanently in Bash:

1. Open `~/.bashrc` in a text editor.
2. Add the line `export PATH="$HOME/my-scripts:$PATH"` to the end of the file.
3. Save the file and restart your terminal or source the file by typing `. ~/.bashrc`.

Now, your system will look in `~/my-scripts` as well when searching for commands to execute.

Including directories in your PATH is a powerful way to streamline your workflow, making it easier to run programs and scripts without navigating to their direct location or typing the full path every time.