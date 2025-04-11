# Workspace tunning

* notebook
  * make swap space 1.5+ times RAM for hibernation feature
  * use ext4 (supports reducing size)

* panels
  * move panels, bottom and side
  * reserve space for hidden panel on the side - settings > workspace
  * bottom panel disable grouping -- right mouse > panel > preferences > Items > windows buttons > edit
    * sorting order - none
    * grouping - never

* terminal
  * do not show menu
  * color black on white
  * disabled scrollbars
  * disable all menu access, disable menu shortcut (f10), disable help

* wm
  * toggle maximize (alt+b) - settings > window manager
  * focus follow mouse - settings > window manager 
  * alt-delete/insert start terminal - settings > keyboard
    * clear alt-delete delete last workspace action - settings > window manager
  * alt-arrow move workspace - settings > window manager
  * disable shadow -> settings > window manager tweaks > compositor
  * desktop icons -> desktop right mouse
  * make things bigger on FullHD
    * ???? settings > appearance -> dpi 120
    * settings > display > 1600x900 (notebook)
  * power manager to tray
  * pulse audio to tray
  * disable zoom desktop -- settings editor > xfwm4 > zoom_desktop
  * update font alias for bookworm, ~/.config/fontconfig/fonts.conf
```
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "urn:fontconfig:fonts.dtd">
<!--
bookworm aliases to match monospace a same font as bullseye
https://forums.debian.net/viewtopic.php?t=154903

## bullseye
$ for i in sans serif mono ; do echo "$i: $(fc-match $i|awk -F\" '{print $2}')" ; done
sans: DejaVu Sans
serif: DejaVu Serif
mono: DejaVu Sans Mono

## bookworm
for i in sans serif mono ; do echo "$i: $(fc-match $i|awk -F\" '{print $2}')" ; done
sans: DejaVu Sans
serif: DejaVu Serif
mono: Noto Sans Mono

-->

<fontconfig>
   <alias>
      <family>monospace</family>
      <prefer><family>DejaVu Sans Mono</family></prefer>
   </alias>
</fontconfig>
```

* apps
  * chrome
  * firefox
  * nftables
  * virtualbox, extpack

* misc
  * pulseaudio/pipewire - in order to use microphone properly, default fallback device must be set properly (cam or headphones)
  * airpods 3gen + asus USB BT-500 + Debian 12 Bookworm - require to install and use `pipewire-pulse`, `libspa-0.2-bluetooth`
    * https://wiki.archlinux.org/title/bluetooth_headset
  * sony wh-ch720n + asus USB BT-500 + Debian 12 Bookworm
    * ```libpipewire-0.3-0:amd64, libpipewire-0.3-common, libpipewire-0.3-modules:amd64, pipewire:amd64, pipewire-bin, pipewire-jack:amd64, pipewire-pulse, pulseaudio, pulseaudio-module-bluetooth, pulseaudio-utils, xfce4-pulseaudio-plugin:amd64, wireplumber```
    * https://gitlab.freedesktop.org/pipewire/pipewire/-/wikis/Troubleshooting#missing-bluetooth-profiles--volume-control--etc
  * remmina - global keyboard settings, update host key from Control_R, which is required to pass to virtualbox when mouse is captured

* notebook
  * tlp
  
  * lid close -> settings > power manager
    * system sleep mode -- battery hibernate, plugged suspend
    * laptop lid -- battery suspend, plugged suspend
