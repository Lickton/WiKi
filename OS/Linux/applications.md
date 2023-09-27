# 应用程序

## 日常使用

### Google-chrome

```bash
$ yay -S google-chrome
```

### Yay

AUR Helper
```bash
$ sudo pacman -S yay
$ # Search for the package
$ yay -s <package>
$ # Install the package
$ yay -S <package>
$ # Uninstall the package
$ yay -Runs <package>
$ # change aur source, if you are using a proxy, just "" is ok
$ yay -aururl "https://..."
```

**注意，清华源已不再维护**

### V2rayA

更多查阅 [v2raya.org](https://v2raya.org)

```bash
$ yay -S v2raya
```

### Flameshot

截图软件

```bash
$ yay -S flameshot
```

i3wm中，在`~/.config/i3/config`中合适位置添加键位绑定
```
bindsym $mod+Shift+a exec flameshot gui
```

### terminal

使用过 kitty ， alacritty ，其中 kitty 支持连体字，alacritty 不支持，但 alacritty 更快（不愧是rust编写的）

```bash
$ yay -S kitty alacritty
```

kitty 的美化参考 [github:dexpota/kitty-themes](https://github.com/dexpota/kitty-themes)   
alacritty 的美化参考 [github:alacritty/alacritty-theme](https://github.com/alacritty/alacritty-theme)   

### fcitx5

中文输入法   

详见[Arch Wiki:Fcitx5](https://wiki.archlinuxcn.org/wiki/Fcitx5)

```bash
$ yay -S fcitx5
```

### obs-studio

录屏软件

```bash
$ yay -S obs-studio
```

可以配合 screenkey ，实时显示键盘输入
```bash
$ yay -S screenkey
```

### motrix

下载器，支持下载HTTP、FTP、BT、Torrent等资源   
[github:agalwood/Motrix](https://github.com/agalwood/Motrix/blob/master/README-CN.md)

```bash
$ yay -S motrix
```

## 美化

### conky

config file at `~/.config/conky/conky.conf`

```
conky.config = {
    own_window_type = 'override',
}
```

现在`conky`会显示在桌面，使用的是默认配置。
