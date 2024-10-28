# How to Make Ubuntu 24.04 LTS Look Like macOS

Transforming your Ubuntu 24.04 LTS desktop to resemble macOS involves customizing themes, icons, cursors, and dock behavior. Follow these steps:

## 1. Install GNOME Tweaks and Extensions

GNOME Tweaks is essential for applying themes and icons, while GNOME Extensions allows for additional customizations.

```bash
sudo apt update
sudo apt install gnome-tweaks gnome-extensions-app
```

After installation, enable the "User Themes" extension to allow loading themes from local directories.

## 2. Download and Install a macOS-Inspired GTK Theme

A popular choice is the WhiteSur GTK theme, which emulates the macOS aesthetic.

```bash
# Install Git if not already installed
sudo apt install git

# Clone the WhiteSur GTK theme repository
git clone https://github.com/vinceliuice/WhiteSur-gtk-theme.git

# Navigate to the theme directory
cd WhiteSur-gtk-theme

# Install the theme
./install.sh
```

This script installs the theme to your local directory. To apply it:

1. Open GNOME Tweaks.
2. Navigate to the "Appearance" tab.
3. Under "Applications," select "WhiteSur" (or "WhiteSur-dark" for the dark variant).

## 3. Install a macOS-Inspired Icon Theme

To complement the GTK theme, install the WhiteSur icon theme:

```bash
# Clone the WhiteSur icon theme repository
git clone https://github.com/vinceliuice/WhiteSur-icon-theme.git

# Navigate to the icon directory
cd WhiteSur-icon-theme

# Install the icons
./install.sh
```

After installation:

1. Open GNOME Tweaks.
2. Go to the "Appearance" tab.
3. Under "Icons," choose "WhiteSur."

## 4. Apply a macOS Cursor Theme

For a cohesive look, apply a macOS cursor theme:

```bash
# Clone the WhiteSur cursors repository
git clone https://github.com/vinceliuice/WhiteSur-cursors.git

# Navigate to the cursors directory
cd WhiteSur-cursors

# Install the cursor theme
./install.sh
```

Then, in GNOME Tweaks:

1. Open the "Appearance" tab.
2. Under "Cursor," select "WhiteSur."

## 5. Customize the Dock

To mimic the macOS dock:

- **Move the Dock to the Bottom:**

  1. Open "Settings."
  2. Navigate to "Appearance."
  3. Set the dock position to "Bottom."

- **Adjust Dock Behavior:**

  For more customization, consider installing the "Dash to Dock" extension:

  ```bash
  # Install the Dash to Dock extension
  sudo apt install gnome-shell-extension-dash-to-dock

  # Enable the extension
  gnome-extensions enable dash-to-dock
  ```

  Configure it using the "Extensions" app to adjust icon sizes and dock length, aligning with the macOS style.

## 6. Set a macOS Wallpaper

Download macOS wallpapers from sources like [macOS Big Sur Official Wallpapers](https://www.apple.com/macos/big-sur-preview/images/overview/hero_bg.jpg). Right-click on the desktop, select "Change Background," and set your desired wallpaper.