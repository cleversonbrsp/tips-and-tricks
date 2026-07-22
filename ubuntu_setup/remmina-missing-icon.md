# Troubleshooting: Remmina Missing Icon on Cinnamon (Snap Install)

## Issue

Remmina (installed via `snap`) shows up in the Cinnamon menu and taskbar with a generic/missing icon, even though the app runs fine.

## Cause

Snap-installed apps get their launcher `.desktop` file generated at `/var/lib/snapd/desktop/applications/`. Most snaps point `Icon=` at an absolute path baked into the snap itself, e.g.:

```
Icon=/snap/spotify/current/usr/share/spotify/icons/spotify-linux-128.png
```

Remmina's `.desktop` file instead uses the freedesktop-style symbolic icon **name**:

```
Icon=org.remmina.Remmina
```

Resolving a symbolic name requires the icon to be present somewhere in the GTK icon theme search path (`$XDG_DATA_DIRS/icons/hicolor/...`, `/usr/share/icons`, or `~/.local/share/icons`). The icon files do exist inside the snap (`/snap/remmina/current/usr/share/icons/hicolor/<size>/apps/org.remmina.Remmina.png`), but that path isn't part of the icon theme search path, and on this system `/var/lib/snapd/desktop/icons` (which is supposed to expose snap icons to the theme) is empty. So Cinnamon/GTK can never resolve the name — this is not a Remmina bug and isn't fixed by updating the snap.

## Solution: Symlink the icons into the user's icon theme directory

This is per-user, doesn't touch anything snapd-managed, and survives snap refreshes.

1. Create the symlinks (adjust sizes if needed — check what's available first):
   ```bash
   SRC=/snap/remmina/current/usr/share/icons/hicolor
   for size in 16x16 22x22 24x24 32x32 48x48 64x64 72x72 96x96 128x128 256x256 512x512; do
     mkdir -p ~/.local/share/icons/hicolor/$size/apps
     ln -sf "$SRC/$size/apps/org.remmina.Remmina.png" ~/.local/share/icons/hicolor/$size/apps/org.remmina.Remmina.png
   done
   mkdir -p ~/.local/share/icons/hicolor/scalable/apps
   ln -sf "$SRC/scalable/apps/org.remmina.Remmina.svg" ~/.local/share/icons/hicolor/scalable/apps/org.remmina.Remmina.svg
   ```

2. Refresh the icon cache:
   ```bash
   gtk-update-icon-cache -f -t ~/.local/share/icons/hicolor
   ```

3. (Optional) Verify the name now resolves:
   ```bash
   python3 -c "
   import gi
   gi.require_version('Gtk', '3.0')
   from gi.repository import Gtk
   theme = Gtk.IconTheme.get_default()
   icon = theme.lookup_icon('org.remmina.Remmina', 48, 0)
   print('resolved to:', icon.get_filename() if icon else None)
   "
   ```

4. Restart Cinnamon so the panel/menu picks up the new icon:
   ```bash
   cinnamon --replace &
   disown
   ```
   (This briefly flashes the screen — normal.) A full logout/login also works.

## Conclusion

The icon assets were never missing — they were just unreachable by name through the GTK icon theme lookup path used by Cinnamon. Symlinking them into `~/.local/share/icons/hicolor/` fixes resolution for the launcher, the menu, and the taskbar/window icon, without needing to touch snapd-managed files or reinstall the app.
