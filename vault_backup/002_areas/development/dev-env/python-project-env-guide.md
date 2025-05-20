---
title: python-project-env-guide
created: 2024-10-30
modified: 2024-10-30
type: process
system:
  - development
tags:
  - process
  - procedure
  - self-dev
  - deployment
  - python
related_docs:
---
# Starting and Stopping Your Coursework

## Quick Reference Guide

### Starting Coursework

#### macOS
- Open Terminal:
  ```bash
  Cmd + Space
  ```
  - Type "Terminal" and press Enter to open it.

#### Ubuntu
- Open Terminal:
  ```bash
  Ctrl + Alt + T
  ```

- Navigate to your course directory:
  ```bash
  cd path/to/your/course_directory
  ```

- Create the virtual environment (only if not created already):
  ```bash
  python3 -m venv cs50p-env
  ```

- Activate the virtual environment:
  ```bash
  source cs50p-env/bin/activate
  ```

### Stopping Coursework
1. Save your work.
2. Deactivate the virtual environment:
   ```bash
   deactivate
   ```

## Detailed Guide

### Starting Your Coursework

#### 1. Open Terminal

##### macOS
- Press `Cmd + Space`, type "Terminal", and press Enter to open Terminal.

##### Ubuntu
- Press `Ctrl + Alt + T` to open Terminal.

#### 2. Navigate to Your Course Directory
- Use the `cd` command to change to your course directory:
  ```bash
  cd path/to/your/course_directory
  ```

#### 3. Create and Activate Your Virtual Environment
- If you haven't already created a virtual environment, do so now, naming it after your course or project. For example:
  ```bash
  python3 -m venv cs50p-env
  ```

- Activate the virtual environment:
  ```bash
  source cs50p-env/bin/activate
  ```

- You should see `(cs50p-env)` at the beginning of your terminal prompt, indicating that the virtual environment is active.

#### 4. Work on Your Course
- You can now start working on your Python scripts, run programs, and install any necessary packages.

### Stopping Your Coursework

#### 1. Save Your Work
- Ensure you save any changes to your Python scripts and other files.

#### 2. Deactivate Your Virtual Environment
- Deactivate the virtual environment:
  ```bash
  deactivate
  ```

- Your terminal prompt should no longer show `(cs50p-env)`.

#### 3. Close Terminal (Optional)
- If you are done with your work session, you can close the terminal.

### Example Project Structure

Here's an example of how your project directory might look:

```
my_python_project/
├── cs50p-env/            # Virtual environment directory
├── main.py               # Main Python script
├── requirements.txt      # Dependencies file
└── README.md             # Project description (optional)
```

