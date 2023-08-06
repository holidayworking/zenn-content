---
title: 'Manjaro Linux セットアップメモ'
emoji: '🧑‍💻'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['Linux', 'Manjaro']
published: true
---

Windows 環境を Beelink EQ12 に移行したので、余った MINISFORUM DeskMini DMAF5 に Manjaro Linux をインストールしてみた。

# インストールメディアの作成

USB メモリーでインストールメディアを作成した。作業した環境はメイン環境の macOS である。

```shell
### USB メモリーのパスを確認
$ diskutil list

### USB メモリーのアンマウント
$ diskutil unmountDisk /dev/disk4

### ISOファイルを USB メモリーに書き込み
$ sudo dd if=manjaro-gnome-22.1.3-230529-linux61.iso of=/dev/disk4
```

# インストール

ぽちぽちと進めた。

# 初期設定

## ミラーサーバーの更新

```shell
$ sudo pacman-mirrors --geoip
```

## システムのアップデート

```shell
$ sudo pacman -Syu
```

## yay のインストール

AUR で公開されているパッケージをインストールするために必要となる。

```shell
$ sudo pacman -S yay
```

## dotfiles のセットアップ

SSH の鍵を 1Password で管理しているので、事前にインストールした。

````shell
$ yay -S 1password
```

そして、GitHub からレポジトリをクローンして、シェルスクリプトを実行した。

```shell
$ mkdir -p ~/src/github.com/holidayworking
$ cd ~/src/github.com/holidayworking
$ git clone --recursive git@github.com:holidayworking/dotfiles.git
$ cd dotfiles
$ ./install.sh
````

端末アプリケーションで、カスタムコマンドを `/usr/bin/fish` に変更してから、端末アプリケーションを再起動後に下記のコマンドを実行した。

```shell
$ curl -fsSL https://git.io/fisher | source && fisher update
```

## トラックパッドの設定

2 本指でクリックしたら、右クリックするようにした。

```shell
$ gsettings set org.gnome.desktop.peripherals.touchpad click-method fingers
```

## 日本語入力のインストール

```shell
$ yay -S fcitx5-mozc-ut
$ sudo pacman -S manjaro-asian-input-support-fcitx5
```

システムを再起動後に Fcitx 5 設定アプリケーションで Mozc を追加した。

## 日本語フォントのインストール

```shell
$ sudo pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji
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

# 各種アプリケーションのインストール

## Microsoft Edge

```shell

$ yay -S microsoft-edge-stable-bin
```

## Visual Studio Code

```shell
$ yay -S visual-studio-code-bin
```
