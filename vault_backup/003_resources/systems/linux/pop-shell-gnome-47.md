---
title: pop-shell-gnome-47
created: 2024-12-05
type: process
system: 
tags:
  - process
  - procedure
  - linux
  - runbook
  - window-tiling
related_docs:
---
### **Simple Guide to Install Pop Shell on GNOME 47**

Pop Shell enhances the tiling and keyboard-driven experience in GNOME. Follow these steps to install and enable it on GNOME 47.

---

### **1. Prerequisites**

Ensure you have the necessary tools and dependencies installed:

```bash
sudo dnf install git make nodejs typescript gcc-c++ gnome-shell-extension-tool gnome-extensions-app
```

---

### **2. Clone the Pop Shell Repository**

Download the latest source code for Pop Shell:

```bash
git clone https://github.com/pop-os/shell.git
cd shell
```

---

### **3. Checkout the Correct Branch**

GNOME 47 requires the `master_noble` branch:

```bash
git checkout master_noble
```

---

### **4. Build and Install Pop Shell**

Build and install the extension locally:

```bash
make local-install
```

---

### **5. Enable the Extension**

Enable Pop Shell using the GNOME Extensions command:

```bash
gnome-extensions enable pop-shell@system76.com
```

---

### **6. Restart GNOME Shell**

- On **X11**: Press `Alt + F2`, type `r`, and press Enter.
- On **Wayland**: Log out and log back in.

---

### **7. Verify the Installation**

Check if Pop Shell is installed and enabled:

```bash
gnome-extensions list | grep pop-shell
```

You should see:

```
pop-shell@system76.com (enabled)
```

---

### **8. Test Pop Shell**

Try the following shortcuts:

- **`Super + Enter`**: Open a terminal.
- **`Super + /`**: Open the launcher.
- **`Super + Arrow Keys`**: Move and resize windows.


> [!TIP] Setup **workspace switching**
> I found workspace switching to be disabled. Navigate to Settings > Keyboard > View and Customize Shortcuts > Navigation > Edit "Switch to workspace on the left/right"


---

### **Optional: Manage Pop Shell via GNOME Extensions App**

Install and open the GNOME Extensions app for easier management:

```bash
sudo dnf install gnome-extensions-app
gnome-extensions-app
```

Locate **Pop Shell** in the app and ensure it’s enabled.

---

### **Uninstalling Pop Shell**

If you need to remove Pop Shell:

1. Navigate to the `shell` directory where you cloned the repository:

```bash
cd shell
make uninstall
```

1. Reset GNOME shortcuts:
    - Open **Settings > Keyboard Shortcuts**.
    - Click **Reset All** in the header bar.

---

This guide ensures Pop Shell is correctly installed and configured for GNOME 47. Enjoy the enhanced tiling experience!