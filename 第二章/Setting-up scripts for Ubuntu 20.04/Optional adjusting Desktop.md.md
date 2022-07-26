# Optional: adjusting Desktop

In GUI desktop, disable auto updates: `Software & updates -> Updates`,
set `Automatically check for updates` to `Never`, untick all checkboxes, click close and click close
again.

```shell script
# Removes nautilus bookmarks and disables lock screen
echo '==> `Ctrl+Alt+T` to start a GUI terminal'
curl -fsSL https://raw.githubusercontent.com/wang-q/ubuntu/master/prepare/2-gnome.sh |
    bash
```
## https://raw.githubusercontent.com/wang-q/ubuntu/master/prepare/2-gnome.sh内容：
```
#!/usr/bin/env bash

# http://superuser.com/questions/244189/bashrc-how-to-know-x-window-is-available-or-not
# https://unix.stackexchange.com/questions/313338/gnome3-how-do-i-remove-favorites-from-dash-via-terminal
#上面两个链接的详细内容看word文件




if [ -n "$DISPLAY" ]; then 

    #如果DISPLAY字符串非空，即如果Windows X 可用，则
    # gsettings get com.canonical.Unity.Launcher favorites    # Unity 启动器的偏好设定

    echo "==> Set favorites  设定喜欢的desktops" 

    gsettings set org.gnome.shell favorite-apps "['ubiquity.desktop', 'org.gnome.Nautilus.desktop', 'gnome-terminal.desktop', 'firefox.desktop', 'gnome-system-monitor.desktop']"
    #上面代码设定了desktops的喜好顺序


    # http://askubuntu.com/questions/177348/how-do-i-disable-the-screensaver-lock
    echo "==> Disable lock screen"

    gsettings set org.gnome.desktop.screensaver lock-enabled false
    #关闭锁屏
    gsettings set org.gnome.desktop.session idle-delay 0 # (0 to disable)
    #关闭息屏，最后0是代表关闭锁屏，将0替换为60代表60s后自动息屏，以此类推

    # http://askubuntu.com/questions/79150/how-to-remove-bookmarks-from-the-nautilus-sidebar/152540#152540
    echo "==> Remove nautilus bookmarks"
    # Nautilus包含一些预定义的书签，这些书签使您可以快速轻松地访问一些常见的文件夹，例如“音乐”和“图片”，以及诸如USB闪存驱动器和网络位置之类的设备。 您可以添加自定义书签，以快速访问经常使用的文件夹。

    echo "enabled=false" > "$HOME/.config/user-dirs.conf"
#将"$HOME/.config/user-dirs.conf"内容替换成输出的字符串"enabled=false"，并应用于系统的所有使用者

#    sed -i 's/\Documents//' "$HOME/.config/user-dirs.dirs" 删除下列书签
#    sed -i 's/\Downloads//' "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Music//'     "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Pictures//'  "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Public//'    "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Templates//' "$HOME/.config/user-dirs.dirs"
    sed -i 's/\Videos//'    "$HOME/.config/user-dirs.dirs"

#    rm -fr "$HOME/Documents" 删除
#    rm -fr "$HOME/Downloads"
    rm -fr "$HOME/Music"
    rm -fr "$HOME/Pictures"
    rm -fr "$HOME/Public"
    rm -fr "$HOME/Templates"
    rm -fr "$HOME/Videos"

    mkdir -p "$HOME/.config/gtk-3.0/"
    echo > "$HOME/.config/gtk-3.0/bookmarks"
# 创建mydir目录，将其写入下面的书签
else
    echo "This script should be execute inside a GUI terminal"
fi
```
