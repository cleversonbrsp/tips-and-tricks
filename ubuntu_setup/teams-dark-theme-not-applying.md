# Troubleshooting: Microsoft Teams Not Following Dark Theme on Cinnamon

## Issue

Microsoft Teams (via the unofficial [teams-for-linux](https://github.com/IsmaelMartinez/teams-for-linux) Electron client) keeps opening in light mode, even though the app's `followSystemTheme` option is enabled by default and the desktop's own theme looks dark. In some cases the in-app **Settings → Appearance and accessibility → Theme** picker is also greyed out and won't let you pick "Dark" manually.

## Cause

`teams-for-linux` decides between light and dark by reading GNOME settings that Cinnamon doesn't keep in sync with its own appearance settings:

```bash
gsettings get org.gnome.desktop.interface color-scheme   # theme preference (light/dark/default)
gsettings get org.gnome.desktop.interface gtk-theme       # GTK theme name, checked for a "dark" substring
```

On **Cinnamon** (and similarly XFCE/MATE), both keys exist (they belong to the `org.gnome.desktop.interface` schema, installed as a GNOME-compat dependency) but neither is updated when you change Cinnamon's own theme. They stay at `'default'` / `'Adwaita'`, so Electron apps that check them (directly or via the `xdg-desktop-portal-gtk` portal) never detect a dark preference.

Even after forcing both keys correctly, detection was still **unreliable across separate launches** — Teams opened in light mode again on at least one boot with both keys already correct and no startup race involved. Treat `followSystemTheme` as best-effort on this setup, not something to depend on.

## Solutions

### Solution 1: Disable "follow system" and set the theme manually (Recommended)

The only combination that has stayed dark reliably across reboots. Skips the flaky detection entirely.

1. Create/edit the config file:
   ```bash
   nano ~/snap/teams-for-linux/current/.config/teams-for-linux/config.json
   ```
2. Add:
   ```json
   { "followSystemTheme": false }
   ```
3. Restart Teams:
   ```bash
   pkill -f teams-for-linux
   ```
4. Reopen the app, then go to **Settings (⚙️) → Appearance and accessibility → Theme → Dark**. With `followSystemTheme` off, the picker is selectable and the choice persists across restarts.

---

### Solution 2: Force the GNOME theme keys (best-effort, helps other Electron apps too)

Worth doing anyway since it benefits every Electron app on the desktop reading these keys (VS Code, etc.), even though it wasn't reliable enough for Teams alone.

1. Set both keys:
   ```bash
   gsettings set org.gnome.desktop.interface color-scheme prefer-dark
   gsettings set org.gnome.desktop.interface gtk-theme 'Adwaita-dark'
   ```
2. Verify:
   ```bash
   gsettings get org.gnome.desktop.interface color-scheme   # 'prefer-dark'
   gsettings get org.gnome.desktop.interface gtk-theme       # 'Adwaita-dark'
   ```
3. Restart Teams:
   ```bash
   pkill -f teams-for-linux
   ```

> Note: these keys can reset back to `'default'` / `'Adwaita'` after desktop updates or a fresh login, so they may need to be re-applied occasionally.

#### Known caveat: startup race on boot

Even with both keys set, Teams can still open light the first time it's launched right after a reboot, if it starts **before** `xdg-desktop-portal-gtk` (the service that answers the theme query) has registered on the session bus:

```bash
ps -o pid,lstart,cmd -C teams-for-linux
ps -o pid,lstart,cmd -C xdg-desktop-portal-gtk
```

If Teams' start time is earlier than the portal's, it asked too early and won't retry — just relaunch it (`pkill -f teams-for-linux`) once the portal is confirmed running. If Teams is ever set to autostart at login, delay it by a few seconds to avoid this race every time.

## Conclusion

The root cause isn't a single Teams bug — it's a combination of (a) Cinnamon not syncing the GNOME theme keys Electron apps rely on, and (b) a startup race with the portal service, and (c) detection simply being flaky on top of both. Forcing the GNOME keys (Solution 2) is worth doing for other Electron apps, but for Teams specifically, disabling `followSystemTheme` and picking the theme manually (Solution 1) is what actually stuck across reboots.
