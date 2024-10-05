# Linux Fresh Install Initial Setup
## Flatpaks
```bash
flatpak install -y \
com.discordapp.Discord \
com.brave.Browser \
ca.desrt.dconf-editor \
org.zotero.Zotero \
org.mozilla.Thunderbird \
com.mattjakeman.ExtensionManager \
org.gnome.DejaDup
```
## Gnome
### Shortcuts
- Settings > Keyboard > View and Customize Shortcuts > Custom Shortcuts > Add Shortcut
  - Lauch Terminal
  - `gnome-terminal` or `kitty`
  - Ctrl + Alt + t

### Extensions
#### Extension Manager Flatpak
Software > Extension Manager

#### Install Third Party Extensions
- Vitals by Vitals@CoreCoding.com
- User Themes
- Impatience

#### Enable System Extensions
- Window List
- Places Status Indicator

### Tweaks
```bash
sudo dnf install -y gnome-tweaks
```
- Fonts > Scaling Factor > 1.20
- Dracula Theme
```bash
wget "https://github.com/dracula/gtk/archive/master.zip" -O temp.zip
unzip temp.zip
rm temp.zip
mv gtk-master Dracula
mkdir ~/.themes/
mv Dracula ~/.themes/
ln -s ~/.themes/Dracula/assets ~/.config/assets
ln -s ~/.themes/Dracula/gtk-4.0/gtk.css ~/.config/gtk-4.0/gtk.css
ln -s ~/.themes/Dracula/gtk-4.0/gtk-dark.css ~/.config/gtk-4.0/gtk-dark.css
gsettings set org.gnome.desktop.interface gtk-theme "Dracula"
gsettings set org.gnome.desktop.wm.preferences theme "Dracula"
```
