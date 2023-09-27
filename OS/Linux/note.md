# 系统备忘录

## xclip

将参数复制到系统剪切版，无需进行输出到`stdout`

```bash
$ # copy current path to clipboard
$ pwd | xclip -r -selection clipboard
$ # with fzf, copy the path of file to clipboard
$ fzf | xclip -r -selection clipboard
```

## fzf

模糊选择器

```bash
$ # select a process and kill it
$ ps aux | fzf | awk '{print $2}' | xargs kill
$ # with fzf, copy the path of file to clipboard
$ fzf | xclip -r -selection clipboard
```

## 多显示器管理 (sddm only)

**笔记本外接显示器**

脚本文件位于 `/etc/X11/xorg.conf.d/sddm_script.sh`
```bash
#!/bin/sh

intern=eDP-1-1
extern0=DP-0
extern1=HDMI-0

if xrandr | grep "$extern0 disconnected"; then
    xrandr --output "$extern0" --off --output "$intern" --auto
else
    xrandr --output "$intern" --off --output "$extern0" --auto
fi
```

此脚本在开机时自动将输出切换至`$extern0`，可以自行修改已满足其他需求（使用xrandr命令查看已连接的显示器）

完成脚本编写后，新建或在`/etc/sddm.conf.d/Xsetup.conf`中添加
```bash
[X11]
DisplayCommand=/etc/X11/xorg.conf.d/sddm_script.sh
```

使其在开机后自动被执行
