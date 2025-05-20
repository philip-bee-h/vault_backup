---
title: py-cli-dot-manager
created: 2024-12-14
type: project
status: 
tags:
  - project
  - area-tag
  - uiowa
related_docs:
---
Packaging the script as a CLI app is straightforward, especially in Python. You can use a tool like `setuptools` to package your script and install it as a system-wide command-line tool. Here’s a step-by-step guide:

---

### **1. Directory Structure**

Create a directory for your project:

```plaintext
dotfiles-manager/
├── dotfiles_manager/
│   ├── __init__.py
│   └── cli.py
├── setup.py
└── README.md
```

- `dotfiles_manager/`: Contains the CLI logic.
- `setup.py`: Contains metadata and installation logic.
- `README.md`: Optional, for project documentation.

---

### **2. Update the Script**

Move your Python code to `dotfiles_manager/cli.py`. Modify it slightly to work as a proper CLI app.

**`cli.py`:**

```python
import os
import argparse
from pathlib import Path

DOTFILES_DIR = Path.home() / "dotfiles"  # Adjust as needed
HOME_DIR = Path.home()

def create_symlink(src, dest):
    try:
        if dest.exists() or dest.is_symlink():
            print(f"Skipping: {dest} already exists")
        else:
            dest.symlink_to(src)
            print(f"Linked: {src} -> {dest}")
    except Exception as e:
        print(f"Failed to link {src} -> {dest}: {e}")


def remove_symlink(dest):
    try:
        if dest.is_symlink():
            dest.unlink()
            print(f"Unlinked: {dest}")
        elif dest.exists():
            print(f"Skipping: {dest} is not a symlink")
        else:
            print(f"Skipping: {dest} does not exist")
    except Exception as e:
        print(f"Failed to unlink {dest}: {e}")


def process_package(package, link=True):
    package_path = DOTFILES_DIR / package
    if not package_path.exists():
        print(f"Package not found: {package}")
        return

    for root, _, files in os.walk(package_path):
        relative_path = Path(root).relative_to(package_path)
        target_dir = HOME_DIR / relative_path

        if link:
            target_dir.mkdir(parents=True, exist_ok=True)

        for file in files:
            src = Path(root) / file
            dest = target_dir / file
            if link:
                create_symlink(src, dest)
            else:
                remove_symlink(dest)


def main():
    parser = argparse.ArgumentParser(description="Dotfiles Manager")
    parser.add_argument("action", choices=["link", "unlink"], help="Action to perform")
    parser.add_argument("package", nargs="?", help="Specific package to process (optional)")
    parser.add_argument("--all", action="store_true", help="Process all packages")

    args = parser.parse_args()

    if args.all:
        for package in DOTFILES_DIR.iterdir():
            if package.is_dir() and not package.name.startswith("."):
                print(f"Processing package: {package.name}")
                process_package(package.name, link=(args.action == "link"))
    elif args.package:
        process_package(args.package, link=(args.action == "link"))
    else:
        print("You must specify a package or use --all")


if __name__ == "__main__":
    main()
```

---

### **3. Add `setup.py`**

The `setup.py` file is used by Python's packaging tools to package and install your application.

**`setup.py`:**

```python
from setuptools import setup, find_packages

setup(
    name="dotfiles-manager",
    version="0.1.0",
    description="A simple CLI tool to manage dotfiles using symlinks",
    author="Your Name",
    author_email="your.email@example.com",
    url="https://github.com/yourusername/dotfiles-manager",
    packages=find_packages(),
    entry_points={
        "console_scripts": [
            "dotfiles-manager=dotfiles_manager.cli:main",
        ],
    },
    install_requires=[],
    python_requires=">=3.6",
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
)
```

---

### **4. Install the CLI**

Run the following command in the project directory to install your CLI:

```bash
pip install .
```

This installs your app and makes the `dotfiles-manager` command available globally. You can now run:

```bash
dotfiles-manager link --all
dotfiles-manager unlink zshrc
```

---

### **5. Add to GitHub or PyPI**

- **GitHub**: Push your project to a GitHub repository for version control.
- **PyPI** (optional): Publish your package to the Python Package Index so others can install it using `pip install dotfiles-manager`.

---

### **6. Testing**

To test locally before publishing:
1. Uninstall the package if installed:
   ```bash
   pip uninstall dotfiles-manager
   ```
2. Install in "editable" mode for testing:
   ```bash
   pip install -e .
   ```
3. Test commands:
   ```bash
   dotfiles-manager link vim
   dotfiles-manager unlink --all
   ```

---

This approach gives you a modular, reusable, and distributable CLI application for managing dotfiles. Let me know if you'd like to enhance this further, such as adding a configuration file, handling `.ignore` rules, or supporting dry-run mode!