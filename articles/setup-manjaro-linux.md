---
title: 'Manjaro Linux セットアップメモ'
emoji: '🧑‍💻'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['Linux', 'Manjaro']
published: true
---

Windows 環境を Beelink EQ12 に移行したので、余った MINISFORUM DeskMini DMAF5 に Manjaro Linux をインストールしてみた。

# インストールメディアの作成

USB メモリーでインストールメディアを作成した。作業した環境はメイン環境の macOS です。

```shell
### USB メモリーのパスを確認
diskutil list

### USB メモリーのアンマウント
diskutil unmountDisk /dev/disk4

### ISOファイルを USB メモリーに書き込み
sudo dd if=manjaro-gnome-22.1.3-230529-linux61.iso of=/dev/disk4
```

# インストール

ぽちぽちと進めた。

# ミラーサーバーの更新

```shell
sudo pacman-mirrors --geoip
```

# システムのアップデート

```shell
sudo pacman -Syu
```

# yay のインストール

AUR で公開されているパッケージをインストールするために必要となる。

```shell
sudo pacman -S yay
```

# 1Password のインストール

```shell
yay -S 1password
```

ログイン時に自動起動させたかったので、`~/.config/autostart/1password.desktop` を作成した。

```plaintext
[Desktop Entry]
Name=1Password
Exec=/opt/1Password/1password --silent %U
Terminal=false
Type=Application
Icon=1password
StartupWMClass=1Password
Comment=Password manager and secure wallet
MimeType=x-scheme-handler/password;
Categories=Office;
```

# dotfiles のセットアップ

dotfiles と言いつつ、 Docker までセットアップしてくれるやつ。

```shell
mkdir -p ~/src/github.com/holidayworking
cd ~/src/github.com/holidayworking
git clone --recursive git@github.com:holidayworking/dotfiles.git
cd dotfiles
./install.sh
```

端末アプリケーションで、カスタムコマンドを `/usr/bin/fish` に変更してから、端末アプリケーションを再起動後に次のコマンドを実行した。

```shell
curl -fsSL https://git.io/fisher | source && fisher update
```

# トラックパッドの設定

2 本指でクリックしたら、右クリックするようにした。

```shell
gsettings set org.gnome.desktop.peripherals.touchpad click-method fingers
```

# キーボードショートカットの設定

```shell
gsettings set org.gnome.settings-daemon.plugins.media-keys screenreader []
gsettings set org.gnome.settings-daemon.plugins.media-keys magnifier []
gsettings set org.gnome.settings-daemon.plugins.media-keys magnifier-zoom-out []
gsettings set org.gnome.settings-daemon.plugins.media-keys magnifier-zoom-in []

gsettings set org.gnome.desktop.wm.keybindings begin-move []
gsettings set org.gnome.desktop.wm.keybindings unmaximize []
gsettings set org.gnome.desktop.wm.keybindings activate-window-menu []
gsettings set org.gnome.desktop.wm.keybindings begin-resize []
gsettings set org.gnome.mutter.keybindings toggle-tiled-right []
gsettings set org.gnome.mutter.keybindings toggle-tiled-left []
gsettings set org.gnome.desktop.wm.keybindings toggle-maximized []

gsettings set org.gnome.shell.keybindings open-application-menu []
gsettings set org.gnome.shell.keybindings toggle-application-view []
gsettings set org.gnome.shell.keybindings toggle-overview []
gsettings set org.gnome.shell.keybindings focus-active-notification []
gsettings set org.gnome.mutter.wayland.keybindings restore-shortcuts []
gsettings set org.gnome.desktop.wm.keybindings panel-run-dialog []
gsettings set org.gnome.settings-daemon.plugins.media-keys logout []
gsettings set org.gnome.settings-daemon.plugins.media-keys screensaver "['<Control><Super>q']"
gsettings set org.gnome.shell.keybindings toggle-message-tray []

gsettings set org.gnome.shell.keybindings show-screen-recording-ui []
gsettings set org.gnome.shell.keybindings show-screenshot-ui "['<Shift><Super>5']"
gsettings set org.gnome.shell.keybindings screenshot-window "['<Shift><Super>4']"
gsettings set org.gnome.shell.keybindings screenshot "['<Shift><Super>3']"

gsettings set org.gnome.desktop.wm.keybindings switch-input-source []
gsettings set org.gnome.desktop.wm.keybindings switch-input-source-backward []

gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-1 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-2 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-3 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-4 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-5 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-6 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-7 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-8 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-9 []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-10 []
gsettings set org.gnome.desktop.wm.keybindings move-to-monitor-right []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-right []
gsettings set org.gnome.desktop.wm.keybindings move-to-monitor-down []
gsettings set org.gnome.desktop.wm.keybindings move-to-monitor-left []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-left []
gsettings set org.gnome.desktop.wm.keybindings move-to-workspace-last []
gsettings set org.gnome.desktop.wm.keybindings move-to-monitor-up []
gsettings set org.gnome.desktop.wm.keybindings cycle-windows []
gsettings set org.gnome.desktop.wm.keybindings cycle-windows-backward []
gsettings set org.gnome.desktop.wm.keybindings switch-panels []
gsettings set org.gnome.desktop.wm.keybindings switch-panels-backward []
gsettings set org.gnome.desktop.wm.keybindings cycle-panels []
gsettings set org.gnome.desktop.wm.keybindings cycle-panels-backward []
gsettings set org.gnome.mutter dynamic-workspaces false
gsettings set org.gnome.desktop.wm.preferences num-workspaces 2
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-1 "['<Control>1']"
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-2 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-3 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-4 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-5 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-6 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-7 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-8 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-9 []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-10 []
gsettings set org.gnome.desktop.wm.keybindings switch-group []
gsettings set org.gnome.desktop.wm.keybindings switch-group-backward []
gsettings set org.gnome.desktop.wm.keybindings cycle-group []
gsettings set org.gnome.desktop.wm.keybindings cycle-group-backward []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-right []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-left []
gsettings set org.gnome.desktop.wm.keybindings switch-to-workspace-last []

gsettings set org.gnome.settings-daemon.plugins.media-keys help []

gsettings set org.gnome.mutter overlay-key ""
```

# xremap のインストール

```shell
yay -S xremap-x11-bin
```

一般ユーザーで起動するために、次のコマンドを実行した。

```shell
sudo gpastswd -a $USER input
echo uinput | sudo tee /etc/modules-load.d/uinput.conf
echo 'KERNEL=="uinput", GROUP="input", TAG+="uaccess"' | sudo tee /etc/udev/rules.d/99-input.rules
```

次の内容で `~/.config/xremap/config.yaml` を作成した。

```yaml
modmap:
  - name: Global
    remap:
      Capslock: Ctrl_L

keymap:
  - name: Global
    application:
      not:
        - Code
        - Hyper
    remap:
      C-a: home
      C-e: end
      Super-x: C-x
      Super-c: C-c
      Super-v: C-v
      Super-z: C-z
      Super-a: C-a
      Super-f: C-f
      Super-s: C-s

  - name: Hyper
    application:
      only:
        - Hyper
    remap:
      Super-x: C-Shift-x
      Super-c: C-Shift-c
      Super-v: C-Shift-v
      Super-z: C-Shift-z
      Super-a: C-Shift-a
      Super-f: C-Shift-f
```

そして、`xremap ~/.config/xremap/config.yaml` を実行後 、期待通りにキーがスワップしていることを確認した。

systemd で管理するために次の内容で `~/.config/systemd/user/xremap.service` を作成した。

```plaintext
[Unit]
Description=xremap

[Service]
KillMode=process
ExecStart=/usr/bin/xremap %h/.config/xremap/config.yaml
ExecStop=/usr/bin/killall xremap
Type=simple
Restart=always

[Install]
WantedBy=default.target
```

そして、有効化した。

```shell
systemctl --user enable xremap.service
systemctl --user start xremap.service

```

# フォントのインストール

```shell
sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji ttf-fira-code ttf-inconsolata
```

[フォント設定/サンプル \- ArchWiki](https://wiki.archlinux.jp/index.php/%E3%83%95%E3%82%A9%E3%83%B3%E3%83%88%E8%A8%AD%E5%AE%9A/%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB) を参考に `~/.config/fontconfig/fonts.conf` を作成した。

```xml
<?xml version='1.0'?>
<!DOCTYPE fontconfig SYSTEM 'fonts.dtd'>
<fontconfig>

<!-- Default font (no fc-match pattern) -->
  <match>
    <edit mode="prepend" name="family">
    <string>Noto Sans</string>
    </edit>
  </match>

<!-- Default font for the ja_JP locale (no fc-match pattern) -->
  <match>
    <test compare="contains" name="lang">
    <string>ja</string>
    </test>
    <edit mode="prepend" name="family">
    <string>Noto Sans CJK JP</string>
    </edit>
  </match>

<!-- Default sans-serif font -->
  <match target="pattern">
    <test qual="any" name="family"><string>sans-serif</string></test>
    <!--<test qual="any" name="lang"><string>ja</string></test>-->
    <edit name="family" mode="prepend" binding="same"><string>Noto Sans</string>  </edit>
  </match>

<!-- Default serif fonts -->
  <match target="pattern">
    <test qual="any" name="family"><string>serif</string></test>
    <edit name="family" mode="prepend" binding="same"><string>Noto Serif</string>  </edit>
    <edit name="family" mode="append" binding="same"><string>IPAPMincho</string>  </edit>
    <edit name="family" mode="append" binding="same"><string>HanaMinA</string>  </edit>
  </match>

<!-- Default monospace fonts -->
  <match target="pattern">
    <test qual="any" name="family"><string>monospace</string></test>
    <edit name="family" mode="prepend" binding="same"><string>Inconsolatazi4</string></edit>
    <edit name="family" mode="append" binding="same"><string>IPAGothic</string></edit>
  </match>

<!-- Fallback fonts preference order -->
  <alias>
    <family>sans-serif</family>
    <prefer>
    <family>Noto Sans</family>
    <family>Open Sans</family>
    <family>Droid Sans</family>
    <family>Ubuntu</family>
    <family>Roboto</family>
    <family>NotoSansCJK</family>
    <family>Source Han Sans JP</family>
    <family>IPAPGothic</family>
    <family>VL PGothic</family>
    <family>Koruri</family>
    </prefer>
  </alias>
  <alias>
    <family>serif</family>
    <prefer>
    <family>Noto Serif</family>
    <family>Droid Serif</family>
    <family>Roboto Slab</family>
    <family>IPAPMincho</family>
    </prefer>
  </alias>
  <alias>
    <family>monospace</family>
    <prefer>
    <family>Inconsolatazi4</family>
    <family>Ubuntu Mono</family>
    <family>Droid Sans Mono</family>
    <family>Roboto Mono</family>
    <family>IPAGothic</family>
    </prefer>
  </alias>

  <dir>~/.fonts</dir>
</fontconfig>
```

# 日本語入力のインストール

```shell
sudo pacman -S manjaro-asian-input-support-fcitx5
yay -S fcitx5-mozc-ut
```

システムを再起動後に Fcitx 5 設定アプリケーションで Mozc を追加した。

# Dock にゴミ箱を表示

```shell
gsettings set org.gnome.shell.extensions.dash-to-dock show-trash true
```

# Ulauncher のインストール

```bash
yay -S ulauncher
```

ホットキーは `Super+Space` に変更した。

# Microsoft Ege のインストール

```shell
yay -S microsoft-edge-stable-bin
```

# Hyper のインストール

```shell
yay -S hyper-bin
```

# Visual Studio Code のインストール

```shell
yay -S visual-studio-code-bin
```

キーボードショートカットは次のように変更した。

```json
[
  {
    "key": "ctrl+a",
    "command": "cursorHome"
  },
  {
    "key": "ctrl+e",
    "command": "cursorEnd"
  },
  {
    "key": "meta+x",
    "command": "editor.action.clipboardCutAction"
  },
  {
    "key": "meta+c",
    "command": "editor.action.clipboardCopyAction"
  },
  {
    "key": "meta+v",
    "command": "editor.action.clipboardPasteAction"
  },
  {
    "key": "meta+z",
    "command": "undo"
  },
  {
    "key": "shift+meta+z",
    "command": "redo"
  },
  {
    "key": "meta+a",
    "command": "editor.action.selectAll"
  },
  {
    "key": "meta+f",
    "command": "actions.find",
    "when": "editorFocus || editorIsOpen"
  },
  {
    "key": "meta+s",
    "command": "workbench.action.files.save"
  },
  {
    "key": "meta+f",
    "command": "workbench.action.terminal.focusFind",
    "when": "terminalFindFocused && terminalHasBeenCreated || terminalFindFocused && terminalProcessSupported || terminalFocus && terminalHasBeenCreated || terminalFocus && terminalProcessSupported"
  },
  {
    "key": "ctrl+f",
    "command": "-workbench.action.terminal.focusFind",
    "when": "terminalFindFocused && terminalHasBeenCreated || terminalFindFocused && terminalProcessSupported || terminalFocus && terminalHasBeenCreated || terminalFocus && terminalProcessSupported"
  },
  {
    "key": "meta+f",
    "command": "workbench.action.terminal.copySelection",
    "when": "terminalTextSelectedInFocused || terminalFocus && terminalHasBeenCreated && terminalTextSelected || terminalFocus && terminalProcessSupported && terminalTextSelected || terminalFocus && terminalTextSelected && terminalTextSelectedInFocused || terminalHasBeenCreated && terminalTextSelected && terminalTextSelectedInFocused || terminalProcessSupported && terminalTextSelected && terminalTextSelectedInFocused"
  }
]
```
