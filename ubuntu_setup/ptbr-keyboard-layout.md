### 1️⃣ Install the Brazilian Portuguese Keyboard Layout  
Ubuntu comes with the **pt-br** layout by default, but if it's missing, install it using:

```bash
sudo apt update
sudo apt install console-setup
```

### 2️⃣ Set the Keyboard Layout  
You can configure the layout through the **GUI** or **terminal**.

#### **GUI Method (Graphical Interface)**
1. Open **Settings**.
2. Go to **Region & Language**.
3. Click on **Manage Installed Languages** (install if missing).
4. Under **Input Sources**, click **Add (+)**.
5. Select **Portuguese (Brazil)**.
6. Click **Add**, then move it to the top if you want it as default.

#### **Terminal Method**
Run the following command:

```bash
setxkbmap -layout br
```

To make it persistent across reboots, edit the keyboard configuration:

```bash
sudo nano /etc/default/keyboard
```

Find the line:

```plaintext
XKBLAYOUT="us"
```

Change it to:

```plaintext
XKBLAYOUT="br"
```

Save the file (**Ctrl + X, Y, Enter**), then apply the changes:

```bash
sudo dpkg-reconfigure keyboard-configuration
```

### 3️⃣ Restart Your System  
After making changes, restart your system for them to take full effect:

```bash
sudo reboot
```