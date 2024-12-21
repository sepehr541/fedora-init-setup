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
org.gnome.World.PikaBackup \
org.localsend.localsend_app \
com.github.wwmm.easyeffects \
it.mijorus.gearlever \
io.github.flattool.Warehouse \
org.videolan.VLC \
org.ghidra_sre.Ghidra \
org.lamport.tla.toolbox \
com.visualstudio.code \
org.telegram.desktop \
com.slack.Slack \
com.spotify.Client \
us.zoom.Zoom \
org.qbittorrent.qBittorrent \
com.bambulab.BambuStudio \
com.protonvpn.www \
com.bitwarden.desktop
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
bat \
gh \
openssl
```

## Gnome
### Pop Shell
```bash
sudo dnf install gnome-shell-extension-pop-shell xprop
```
- logout and log back in

### Shortcuts
- Settings > Keyboard > View and Customize Shortcuts > Custom Shortcuts > Add Shortcut
  - Launch Terminal
  - `gnome-terminal` or `kitty`
  - Ctrl + Alt + t

### Extensions
#### Third Party Extensions
- [Vitals](https://extensions.gnome.org/extension/1460/vitals/)
- [User Themes](https://extensions.gnome.org/extension/19/user-themes/)
- [EasyEffects Preset Selector](https://extensions.gnome.org/extension/4907/easyeffects-preset-selector/)
- [Just Perfection](https://extensions.gnome.org/extension/3843/just-perfection/)
- [Start Overlay in Application View](https://extensions.gnome.org/extension/5040/start-overlay-in-application-view/)
- [GSConnect](https://extensions.gnome.org/extension/1319/gsconnect/)
- [Fedora Linux Update Indicator](https://extensions.gnome.org/extension/6406/fedora-linux-update-indicator/)

#### System Extensions
- Window List
- Places Status Indicator

### Tweaks
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
gsettings set org.gnome.shell.extensions.user-theme name 'Dracula'
```
### Fractional Scaling
```bash
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
# logout and login to see options
```


## Kitty
### Dracula Theme
```bash
wget "https://github.com/dracula/kitty/archive/master.zip" -O temp.zip
unzip temp.zip
rm temp.zip
mkdir -p ~/.config/kitty/
touch ~/.config/kitty/kitty.conf
cp kitty-master/dracula.conf kitty-master/diff.conf ~/.config/kitty/
echo "include dracula.conf" >> ~/.config/kitty/kitty.conf
echo "font_size 14" >> ~/.config/kitty/kitty.conf
echo "shell zsh" >> ~/.config/kitty/kitty.conf
```
### Keyboard shortcuts
- split window: `ctrl + shift + enter`
- change window layout `ctr + shift + l`
- change focus between windows `ctrl + shift + [` and `ctrl + shift + ]`

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
- add container emoji to prompt to default Oh My Zsh
  ```text
    # in .zshrc
    update_prompt_in_container() {
    if [[ -n "$CONTAINER_ID" && "$PROMPT" != *"📦"* ]]; then
        PROMPT="📦 $PROMPT"
    elif [[ -z "$CONTAINER_ID" && "$PROMPT" == *"📦"* ]]; then
        PROMPT="${PROMPT/📦 /}"  # Remove emoji when not in container
    fi
    }
    precmd_functions+=(update_prompt_in_container)
  ```
- add zsh-autosuggestions plugin
  ```bash
  # clone the plugin
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```
  ```bash
  nano ~/.zshrc
  ```
  ```bash
  plugins=( 
    # other plugins...
    zsh-autosuggestions
  )
  ```
- syntax highlighting for `less` using `bat` (or `batcat` on Ubuntu?)
  ```zsh
  nano ~/.zshrc
  ```
  ```zsh
  export LESSOPEN='|bat --paging=never --color=always %s'
  ```
- plugins
  ```zsh
  plugins=(
  git
  zsh-autosuggestions
  dnf
  )
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
  .browser-titlebar {
    visibility: collapse;
  }
  ```
- in URL bar > about:config
- search for `toolkit.legacyUserProfileCustomizations.stylesheets` and set it to `true`

## Kernel Parameters
### AMD GPU
- Fedora Workstation
  ```bash
  sudo nano /etc/default/grub
  ```
- Fedora Silverblue / Bluefin (immutable)
  ```bash
  rpm-ostree kargs --editor
  ```
```bash
# append amdgpu.sg_display=0
# append amdgpu.abmlevel=0 to fix panel brightness change when changing power profile
# GRUB_CMDLINE_LINUX_DEFAULT="[...] quiet splash amdgpu.sg_display=0 amdgpu.abmlevel=0"
```
```bash
amdgpu.sg_display=0 amdgpu.abmlevel=0
```
- Fedora Workstation
  ```bash
  sudo grub2-mkconfig
  ```

## CA certificates
- certs are under `/etc/ssl/certs/`
- e.g., `/etc/ssl/certs/Digicert_Global_Root_CA.crt`

## Caps Lock as Ctrl
- GNOME Tweaks > Keyboard > Additional Layout Options > Ctrl Position > Caps Lock as Ctrl

## Mount Google Drive via `rclone`
[How to Use Google Drive in Linux](https://www.baeldung.com/linux/google-drive-guide#2-rclone)
```
sudo dnf install -y rclone
rclone config
# choose google drive (17 or 18)
# leave everything empty
mkdir GDrive
rclone mount --daemon --vfs-cache-mode full google-drive:/ ./GDrive
```
### setup systemd service to automount on boot
[modified version of this post's script](https://www.guyrutenberg.com/2021/06/25/autostart-rclone-mount-using-systemd/)
```
[Unit]
Description=Mount Google Drive (rclone)
AssertPathIsDirectory=%h/GDrive
# Make sure we have network enabled
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/rclone mount --vfs-cache-mode full google-drive: %h/GDrive
Restart=on-failure
RestartSec=15

[Install]
# Autostart after reboot
WantedBy=default.target
```
```bash
sudo systemctl --user daemon-reload
sudo systemctl --user enable --now rclone-dropbox
```
