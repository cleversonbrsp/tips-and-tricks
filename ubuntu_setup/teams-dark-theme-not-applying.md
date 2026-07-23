# Troubleshooting: Microsoft Teams Not Following Dark Theme on Cinnamon

## Issue

Microsoft Teams (via the unofficial [teams-for-linux](https://github.com/IsmaelMartinez/teams-for-linux) Electron client) keeps opening in light mode, even though the app's `followSystemTheme` option is enabled by default and the desktop's own theme looks dark.

## Cause

`teams-for-linux` decides between light and dark by reading the GNOME setting:

```bash
gsettings get org.gnome.desktop.interface color-scheme
```

On **Cinnamon** (and similarly on XFCE/MATE), this key exists but is **not synced** with the desktop's own appearance settings, since it's a GNOME-specific gsetting. It stays at `'default'`, so Electron apps that check it via the freedesktop portal (Teams, VS Code, etc.) never detect a dark preference, regardless of what theme Cinnamon itself is set to.

## Solutions

### Solution 1: Force the GNOME color-scheme key (Recommended)

This keeps `followSystemTheme` working and fixes every Electron app relying on the same key, not just Teams.

1. Set the key manually:
   ```bash
   gsettings set org.gnome.desktop.interface color-scheme prefer-dark
   ```
2. Verify it was applied:
   ```bash
   gsettings get org.gnome.desktop.interface color-scheme
   # should print 'prefer-dark'
   ```
3. Restart Teams so it picks up the change on launch:
   ```bash
   pkill -f teams-for-linux
   ```
   Then reopen the app normally.

> Note: this key can be reset back to `'default'` by desktop updates or a fresh login, so it may need to be re-applied occasionally.

---

### Known caveat: still opens light right after a fresh boot/login

Even with `color-scheme` correctly set to `prefer-dark`, Teams can still open in light mode the first time it's launched after a reboot. This is a **startup race** between `teams-for-linux` and the `xdg-desktop-portal-gtk` service (the backend that actually answers the theme query):

```bash
# Compare start times:
ps -o pid,lstart,cmd -C teams-for-linux
ps -o pid,lstart,cmd -C xdg-desktop-portal-gtk
```

If Teams started *before* `xdg-desktop-portal-gtk` registered on the session bus, it asked for the theme too early, got no answer, and doesn't retry later in the session. The fix is simply to relaunch Teams once the portal is confirmed running:

```bash
pkill -f teams-for-linux
```

Then reopen the app — it will read the theme correctly this time. If Teams is ever configured to autostart at login, delay it by a few seconds to avoid hitting this race every time.

---

### Solution 2: Disable "follow system" and set the theme manually

Use this if Solution 1 doesn't stick, or if you'd rather control Teams' theme independently of the OS.

1. Create/edit the config file:
   ```bash
   nano ~/snap/teams-for-linux/current/.config/teams-for-linux/config.json
   ```
2. Add:
   ```json
   { "followSystemTheme": false }
   ```
3. Restart Teams, then go to **Settings (⚙️) → Appearance and accessibility → Theme → Dark** inside the app itself.

## Conclusion

The root cause isn't a Teams bug — it's a gap between Cinnamon's theme settings and the GNOME gsetting that Electron apps use to detect dark mode. Forcing `color-scheme` to `prefer-dark` (Solution 1) is the quickest fix and benefits any other Electron app on the same desktop with the same symptom.
