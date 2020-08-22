---
layout: post
title: Suckless st vs urxvt [unix]
---

## Introduction
Urxvt is a common terminal emulator that works fine until you want to use unicode. Although the emulator can
handle unicode, the rendering is a known problem. St on the other hand handles unicode perfectly.

## Suckless philosophy
The (Suckless philosophy)[https://suckless.org/philosophy/] is admirable. It aims at developing light software that the
user themselves customize to their needs. The code is minimalistic and the configs are embedded in the
installation.

## Installing st
Download st from [Suckless](https://st.suckless.org/) and extract it into $HOME/st. In the README it says how to install it.
The installation requires Xlib header files, in Debian based distributions you need,

```
sudo apt-get install libxft-dev
```

If there is something else missing, you can potentially find that with, e.g.,

```
sudo apt-get install apt-file
sudo apt-file update
apt-file search X11/Xlib.h
sudo apt install libx11-dev
```

Now for the installation. Extract the tar.gz file, and then apply the patches that you want (which can be
downloaded from st's page), and then install.

```
tar xf st-0.8.2.tar.gz
cd st-0.8.2
patch -Np1 -i ~/st/patches/st-alpha-0.8.2.diff
patch -Np1 -i ~/st/patches/st-clipboard-0.8.2.diff
patch -Np1 -i ~/st/patches/st-scrollback-0.8.2.diff
sudo make clean install
st
```
I have chosen not to install a theme, which you can do with, e.g.,
```
patch -Np1 -i ~/st/patches/st-solarized-both-20190128-3be4cf1.diff
```

Instead I changed the colors in the config.h file and resinstalled.

For unicode characters in st you want to install a "powerline patched font", this is , e.g., necessary if you want your font to be compatilbe with Vim Airline which uses PowerlineSymbols unicode. [Nerds fonts](https://github.com/ryanoasis/nerd-fonts) have several alternatives for fonts. I use the font called [Hack](https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/Hack), and it can be installed with,

```
sudo apt-get install fonts-hack-ttf
```
