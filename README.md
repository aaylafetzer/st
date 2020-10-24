# Luke's build of st - the simple (suckless) terminal
## Featuring stylistic sets patch by and settings for wundrweapon

The [suckless terminal (st)](https://st.suckless.org/) with some additional features that make it literally the best
terminal emulator ever:

## Unique features (using dmenu)

- **Follow urls** by pressing Alt+L
- **Copy urls** in the same way with Alt+Y
- **Copy the output of commands** with Alt+O

## Bindings for

- **Scrollback** with Alt+↑/↓ or Alt+PageUp/Down or Shift while scrolling the mouse
  + OR **vim bindings**: scroll up/down in history with Alt+K and Alt+J. Faster with Alt+U/D
- **Zoom/change font size**: same bindings as above, but holding down shift as well. Alt+Home returns to default
- **Copy text** with Alt+C, **paste** is Alt+V or Shift+Insert

## Pretty stuff

- Compatibility with `Xresources` and `pywal` for dynamic colors
- Default [gruvbox](https://github.com/morhetz/gruvbox) colors otherwise
- Transparency/alpha, which is also adjustable from your `Xresources`
- Default font is system "mono" at 14pt, meaning the font will match your system font
- font2 default is Fira Code at 16pt because it is superior. fite me.

## Other st patches

- Vertcenter
- Scrollback
- font2
- Ligatures and stylistic sets

## Installation

Just clone the repo and `make install`:
```
git clone https://gitlab.com/wundrweapon/st.git
cd st
sudo make install
```

Obviously, `make` is required to build as are the following development packages (suffixed `-devel` on Void Linux).
You'll also need their base packages for execution:

- `fontconfig` for the default build, for your system monospace font
- `libX11` and `libXft` to have an actually functioning window
- `freetype` v2.0+ because… I dunno, it's used in compiling
- `harfbuzz` for ligatures

I'm sure that list is incomplete, so please send an issue if you find any dependencies not listed!  
I plan to write an xbps template for this Eventually™ so be on the lookout for that. This file will be updated
accordingly at that time.

If you want to install on OpenBSD, be sure to edit `config.mk` and remove `-lrt` from `$LIBS` before compiling.

Lastly, you need a composite manager (`xcompmgr`, `picom`, etc.) running if you want transparency.

## How to configure dynamically with Xresources

For many key variables, this build of `st` will look for X settings set in either `~/.Xdefaults` or `~/.Xresources`.
You must run `xrdb` on one of these files to load the settings.

For example, you can define your desired fonts, transparency or colors:

```xresources
*.font: Liberation Mono:pixelsize=12:antialias=true:autohint=true;
*.alpha: 0.9
*.background: #1d2021
*.cursorColor: #00bbbb
```

The `alpha` value (for transparency) goes from `0` (transparent) to `1` (opaque).  
The background and cursor colors are RGB prefixed with `#`

### Colors

To be clear about the color settings:

- This build will use gruvbox colors by default and as a fallback
- If there are `Xresources` colors defined, those will take priority
- But if `wal` has run in your session, its colors will take priority

Note that when you run `wal`, it will negate the transparency of existing windows, but new windows will continue with
the previously defined transparency.

## Notes on Emojis and Special Characters

If st crashes when viewing emojis, there are two solutions:

1. Change your emoji font. I have no idea what `st` is using by default on my desktop, but I know that it doesn't have
   color emoji. The colors are the culprit
2. Install the BGRA patch for libXft. [Here](https://gitlab.freedesktop.org/xorg/lib/libxft/-/merge_requests/1)'s the
   relevant merge request on the FreeDesktop project. The team are concerned with the code's scalability, so it doesn't
   look like it'll merge into the library any time soon. `pacman` users can find
   [a build with this patch](https://aur.archlinux.org/packages/libxft-bgra/) on the AUR, and others may want to use
   [this little script](https://github.com/uditkarode/libxft-bgra) I found on GitHub

Note that some special characters may appear truncated if too wide. You might want to manually set your prefered
emoji/special character font to a lower size in `config.h` to avoid this. By default, Fira Code is used with a larger
size than the usual text, so this could exacerbate the problem. I'm not sorry.

## Bug reports

Over 95% of bugs will be either in libXft (because of color emoji, as described) or in the patches Luke added. Test
[his build](https://gitlab.com/LukeSmithxyz/st) first. If the bug persists, bother him not me. If the bug only exists
here, even on a fresh build without using Xresources or whatever, then it's my fault and you are welcome to file an
issue on [the GitLab page](https://gitlab.com/wundrweapon/st).
