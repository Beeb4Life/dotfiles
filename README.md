<h2 align="center">My DWM Dotfiles</h2>

Ever since ive started using Linux and Watching videos on different window managers DWM caught my eye the most. It was lightweight and pretty quick since it was only few lines of code

patching was tough but it soon got easier after working a lot with it and now ive finally built a setup im happy with.

;-; although i still have to figure a lot of things out to make it better.
<br>


## Screenshots

![](https://github.com/Beeb4Life/dotfiles/blob/main/assets/rice1.png)

![](https://github.com/Beeb4Life/dotfiles/blob/main/assets/rice2.png)

![](https://github.com/Beeb4Life/dotfiles/blob/main/assets/rice3.png)

## My Build

- **dwm** (patched)
- **st** (patched)
- **dmenu** (patched)
- **dwmblocks-async**
- Simple scripts for statusbar and pywal.
- A few useful configs maybe [picom]

Everything here is part of *my personal workflow*.  
Feel free to use anything as a **reference**, cloning it directly might not work on your setup.

## installation, setup:

* i use sddm (silent-sddm [aur] ) but you will have to make a dwm.desktop file if remember correctly

* i suggest you git clone it from suckless' website and use my configs as reference also use your own choice of patches to make the software your own.
  - [dwm](https://dwm.suckless.org/)
  - [st](https://st.suckless.org/)
  - [dmenu](https://tools.suckless.org/dmenu/)
  - [dwmblocks](https://github.com/UtkarshVerma/dwmblocks-async)

* Configure settings (fonts, bindings, gap pixels, etc) in **config.def.h** before compiling.
  - Defaults: Mod is bound to the windows key
  - ```mod + enter``` to open terminal
  - ```mod + c``` to close window
  - ```mod + shift + q``` to fully exit

## colors, other stuff:
If you aren't using ```~/.Xresources``` with or without [pywal16](https://github.com/eylles/pywal16), default color palette is a normal dwm colors.


I have wal generate a template containing [dwm Xresource strings](https://dwm.suckless.org/patches/xrdb/). Then, I merge it with wal's auto-generated Xresources file, using ```xrdb -merge```.


```~/.config/wal/templates/xrdb_extra```
```
dwm.normbordercolor: {color0}
dwm.normbgcolor: {color0}
dwm.normfgcolor: {color4}
dwm.selbordercolor: {color8}
dwm.selbgcolor: {color4}
dwm.selfgcolor:  {color0}
```

## Acknowledgements

- Statusbar scripts come from the work of **breadonpenguins**, **Luke Smith**, and **arkboi** — all credit goes to them.
- To learn more about dwm check them out:- [BreadonPenguins](https://www.youtube.com/@BreadOnPenguins) , [arkboi](https://www.youtube.com/@arkbooi) ,
[Luke Smith](https://www.youtube.com/@LukeSmithxyz), [Mashed](https://www.youtube.com/@MashedLinux).