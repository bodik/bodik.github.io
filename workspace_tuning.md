# Workspace tunning

* notebook
  * make swap space 1.5+ times RAM for hibernation feature
  * use ext4 (supports reducing size)

* panels
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
  * toggle maximize - settings > window manager
  * focus follow mouse - settings > window manager 
  * alt-delete/insert start terminal - settings > keyboard
  * alt-arrow move workspace - settings > window manager
  * disable shadow -> settings > window manager tweaks > compositor
  * desktop icons -> desktop right mouse
  * make things bigger on FullHD
    * ???? settings > appearance -> dpi 120
    * settings > display > 1600x900 (notebook)
  * power manager to tray
  * pulse audio to tray
  * disable zoom desktop -- settings editor > xfwm4 > zoom_desktop

* apps
  * chrome
  * firefox
  * nftables
  * virtualbox, extpack

* misc
  * pulseaudio - in order to use microphone properly, default fallback device must be set properly (cam or headphones)
  * remmina - global keyboard settings, update host key from Control_R, which is required to pass to virtualbox when mouse is captured
  * ??? xfce 4.16 - "us,cz" must be set in /etc/default/keyboard, then keyboard switcher works as expected (might require re-adding on panel)

* notebook
  * tlp
  
  ~~* lid close -> settings > power manager~~
    ~~* system sleep mode -- battery hibernate, plugged suspend~~
    ~~* laptop lid -- battery suspend, plugged suspend~~
  
  * T14s Gen2 power/sleep config, many of thinkpad laptops has sleep issues, sometimes they get fixed by bios updates
    * bug1: "modern sleep" leaves fan running which might damage device in the bag
    * bug2: S3 sleep is not handled well by touchpad, which became erratic after resume
    * solution:
      * use S3 sleep in bios
      * lid close -> settings > power manager
        * system sleep mode - battery hibernate, plugged hibernate
        * laptop lip - battery hibernate, plugged hibernate
      * https://forums.lenovo.com/t5/Fedora/Touchpad-become-lag-and-Jumping-after-S3-suspend/m-p/5081818
      * https://forums.lenovo.com/t5/ThinkPad-T400-T500-and-newer-T-series-Laptops/Thinkpad-T14s-Gen-2-Intel-touchpad-erratic-after-sleep/m-p/5114517
