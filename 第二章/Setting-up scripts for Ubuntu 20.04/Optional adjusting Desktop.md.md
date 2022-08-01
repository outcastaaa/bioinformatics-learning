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

