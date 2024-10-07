# Fedora Workstation Fresh Install Initial Setup
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
org.localsend.localsend_app \
com.github.wwmm.easyeffects
```

## DNF packages
```bash
sudo dnf install -y \
zsh \
kitty \
distrobox \
emacs \
ranger \
gnome-tweaks \
grub2-common \
perf
```

## Gnome
### Shortcuts
- Settings > Keyboard > View and Customize Shortcuts > Custom Shortcuts > Add Shortcut
  - Launch Terminal
  - `gnome-terminal` or `kitty`
  - Ctrl + Alt + t

### Extensions
#### Third Party Extensions
- [Vitals](https://extensions.gnome.org/extension/1460/vitals/)
- [User Themes](https://extensions.gnome.org/extension/19/user-themes/)
- [Impatience](https://extensions.gnome.org/extension/277/impatience/)
- [EasyEffects Preset Selector](https://extensions.gnome.org/extension/4907/easyeffects-preset-selector/)

#### System Extensions
- Window List
- Places Status Indicator

### Tweaks
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
### Fractional Scaling
```bash
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
# logout and login to see options
```


## Kitty
```bash
wget "https://github.com/dracula/kitty/archive/master.zip" -O temp.zip
unzip temp.zip
rm temp.zip
mkdir -p ~/.config/kitty/
touch ~/.config/kitty/kitty.conf
cp kitty-master/dracula.conf kitty-master/diff.conf ~/.config/kitty/
echo "include dracula.conf" >> ~/.config/kitty/kitty.conf
echo "font_size 14" >> ~/.config/kitty/kitty.conf
```

## Zsh
- Install Oh-my-zsh
  ```bash
  sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
  ```
- Set alias for kitty ssh
  ```bash
  echo 'alias s="kitten ssh"' >> ~/.zshrc
  source ~/.zshrc
  ```

## SSH Keys
```bash
mkdir -p ~/.ssh
unzip ssh.zip -d ~/.ssh
# add "IgnoreUnknown UseKeychain" to the top of .ssh/config
```

## Tailscale
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

## Enrol Fingerprint
Settings > System > Users > Fingerprint Login

## Emacs
### Config
```bash
cd
git clone git@github.com:sepehr541/.emacs.d.git
```
### TreeSitter langs
```bash
# Download latest release
curl -s https://api.github.com/repos/emacs-tree-sitter/tree-sitter-langs/releases/latest \
| grep -oP 'https.*tree-sitter-grammars-linux.*gz' \
| xargs wget

# extract to tree-sitter
# rename all files to libtree-sitter-<LANG>.so
mkdir -p tree-sitter && \
tar -xzf tree-sitter.tar.gz -C tree-sitter && \ 
find tree-sitter -type f -name "*.so" | while read -r file; do \
    lang=$(basename "$file" .so); \
    mv "$file" "$(dirname "$file")/libtree-sitter-$lang.so"; \
done

# move to .emacs.d
mv tree-sitter ~/.emacs.d/
```

## EasyEffects Preset
```bash
wget https://gist.githubusercontent.com/cab404/aeb2482e1af6fc463e1154017c566560/raw/3d40d870e4b496b0dc029fff50544d7d31ba5992/Cab's%2520Fav.json
```

## FireFox
### Remove Titlebar + Tabs
≡ > Help > More troubleshooting information > Application Basics > Profile Dierctory > Open Directory
- create a directory called `chrome`
- create a file falled `userChrome.css`
  ```css
  #titlebar {
    visibility: collapse;
  }
  ```
- in URL bar > about:config
- search for `toolkit.legacyUserProfileCustomizations.stylesheets` and set it to `true`

## Kernel Parameters
### AMD GPU
```bash
sudo nano /etc/default/grub
# append amdgpu.sg_display=0
# GRUB_CMDLINE_LINUX_DEFAULT="[...] quiet splash amdgpu.sg_display=0"
#sudo update-grub
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

## CA certificates
- certs are under `/etc/ssl/certs/`
- e.g., `/etc/ssl/certs/Digicert_Global_Root_CA.crt`
