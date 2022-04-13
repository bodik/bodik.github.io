# Workspace tunning

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
  * suspend on lid close -> settings > power manager
    * system sleep mode -- suspend
    * laptop lid -- suspend 
