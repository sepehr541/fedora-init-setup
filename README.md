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
org.gnome.DejaDup \
org.localsend.localsend_app
```
## Gnome
### Shortcuts
- Settings > Keyboard > View and Customize Shortcuts > Custom Shortcuts > Add Shortcut
  - Lauch Terminal
  - `gnome-terminal` or `kitty`
  - Ctrl + Alt + t

### Extensions
#### Third Party Extensions
- Vitals by Vitals@CoreCoding.com
- User Themes
- Impatience

#### System Extensions
- Window List
- Places Status Indicator

```bash
# Install Third Party Extensions
gnome-extensions install Vitals@CoreCoding.com
sudo dnf install gnome-shell-extension-user-theme
gnome-extensions install impatience@gfxmonk.net

# Enable Third Party Extensions
gnome-extensions enable Vitals@CoreCoding.com
gnome-extensions enable user-theme@gnome-shell-extensions.gcampax.github.com
gnome-extensions enable impatience@gfxmonk.net

# Enable System Extensions
gnome-extensions enable window-list@gnome-shell-extensions.gcampax.github.com
gnome-extensions enable places-menu@gnome-shell-extensions.gcampax.github.com
```

### Tweaks
```bash
sudo dnf install -y gnome-tweaks
```
- Fonts > Scaling Factor > 1.20
- Dracula Theme
```bash
sudo dnf install -y gnome-shell-extension-user-theme
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
gsettings set org.gnome.shell.extensions.user-theme name 'Dracula'
```

## DNF packages
```bash
sudo dnf install -y \
zsh \
kitty \
distrobox \
emacs \
ranger
```

## Kitty
```bash
wget "https://github.com/dracula/kitty/archive/master.zip" -O temp.zip
unzip temp.zip
rm temp.zip
mkdir -q ~/.config/kitty/
touch ~/.config/kitty/kitty.conf
cp kitty-master/dracula.conf kitty-master/diff.conf ~/.config/kitty/
echo "include dracula.conf" >> ~/.config/kitty/kitty.conf
echo "font_size 14" >> ~/.config/kitty/kitty.conf
```
