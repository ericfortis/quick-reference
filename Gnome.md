# Gnome

## Key Remap
Using Gnome Tweaks (needs installation)
- Alt/Win key behavior &rarr;  **Ctrl is mapped to Alt; Alt is mapped to Win**
- Caps Lock behavior &rarr;  **Caps Lock is also a Ctrl**


## macOS Fonts
Then, in Gnome:
```shell
mkdir -p ~/.fonts
cp $UXREPO/Shared/DevSetup/Fonts/* ~/.fonts
```


## Segoe UI as system font
```shell
mkdir -p ~/work/forks
cd ~/work/forks
git clone https://github.com/mrbvrz/segoe-ui-linux
cp segoe-ui-linux/font/*.ttf ~/.fonts 
```

Then in `gnome-tweaks` change the system font to **Segoe UI**.

## Font cache
```shell
fc-cache -f -v
```


## Screenshots (png)
```shell
gsettings set org.gnome.gnome-screenshot default-file-type 'png'
gsettings set org.gnome.gnome-screenshot include-pointer false
gsettings set org.gnome.gnome-screenshot include-icc-profile true
```

```shell
cat >> ~/.config/mpv/mpv.conf << EOF
screenshot-format="png"
screenshot-png-compression=0
EOF
```
