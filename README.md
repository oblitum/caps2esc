# caps2esc

_Transforming the most useless key **ever** in the most useful one._
<sub>_For vi/Vim/NeoVim addicts at least_.</sub>

# WARNING

**The project now lives on https://gitlab.com/interception/linux/plugins/caps2esc
as an [Interception Tools][interception-tools] plugin. This repository is now
frozen.**

---

<a href="http://www.catonmat.net/blog/why-vim-uses-hjkl-as-arrow-keys/">

![ADM-3A terminal](http://www.catonmat.net/images/why-vim-uses-hjkl/lsi-adm3a-full-keyboard.jpg)

</a>

## What is it?

- **Put what's useless in its place**  
  <sub>_By moving the CAPSLOCK function to the far ESC location_</sub>
- **Make what's useful comfortably present, just below your Pinky**  
  <sub>_By moving both ESC and CTRL functions to the CAPSLOCK location_</sub>

## Why?!

Because CAPSLOCK is just "right there" and making it CTRL when key-chording and
ESC when pressed alone is quite handy, specially in vi.

## Dependencies

- [libevdev][]

## Building

`gcc caps2esc.c -o caps2esc -I/usr/include/libevdev-1.0 -levdev -ludev`

## Execution

The following daemonized sample execution increases the application priority
(since it'll be responsible for a vital input device, just to make sure it stays
responsible):

`sudo nice -n -20 ./caps2esc >caps2esc.log 2>caps2esc.err &`

## Installation

I'm maintaining an Archlinux package on AUR:

- <https://aur.archlinux.org/packages/caps2esc>

It wraps the executable in a systemd service that can be easily started, stopped
and enabled to execute on boot.

## How it works

Executing `caps2esc` without parameters (with the necessary privileges to access
input devices) will make it monitor any devices connected (or that gets
connected) that produces CAPSLOCK or ESC events.

Upon detection it will fork and exec itself now passing the path of the detected
device as its first parameter. This child instance is then responsible for
producing an uinput clone of such device and doing the programmatic keymapping
of such device until it disconnects, at which time it ends its execution.

## Caveats

As always, there's always a caveat:

- It will "grab" the detected devices for itself.
- If you tweak your key repeat settings, check whether they get reset.  
  Please check [this report][key-repeat-fix] about the resolution.

## History

I can't recall when I started using CAPSLOCK as both ESC and CTRL but it has
been quite some time already. It started when I was on OS X where it was quite
easy to achieve using the [Karabiner][], which already provides an option to
turn CTRL into CTRL/ESC (which can be coupled with OS X system settings that
turn CAPSLOCK into CTRL).

Moving on, permanently making Linux my home, I searched and tweaked a similar
solution based on [xmodmap][] and [xcape][]:

- <https://github.com/alexandre/caps2esc>

It's a simple solution but with many annoying drawbacks I couldn't stand in the
end:

- It resets any time a device change happens (bluetooth, usb, any) or the
  laptop lid is closed or when logging off and needs to be re-executed.
- It depends on [X][]. Doesn't work on TTY (bare terminal based machine,
  CTRL-ALT F2, etc).

Meanwhile on Windows land, I had a definitive solution based on my
[Interception library][interception] that always works perfectly, no hiccups.

It made me envy enough, so I ported the
[Windows Interception caps2esc][caps2esc-windows] sample to Linux based upon
evdev, udev and uinput.

## License

<a href="http://www.gnu.org/copyleft/gpl.html">

![GPL v3](https://www.gnu.org/graphics/gplv3-127x51.png)

</a>

Copyright © 2016 Francisco Lopes da Silva.

[caps2esc-windows]: https://github.com/oblitum/Interception/blob/master/samples/caps2esc/caps2esc.cpp
[libevdev]: https://www.freedesktop.org/software/libevdev/doc/latest/index.html
[karabiner]: https://pqrs.org/osx/karabiner/
[xmodmap]: https://www.x.org/releases/X11R7.7/doc/man/man1/xmodmap.1.xhtml
[xcape]: https://github.com/alols/xcape
[x]: https://www.x.org
[interception]: https://github.com/oblitum/Interception
[key-repeat-fix]: https://github.com/oblitum/caps2esc/issues/1
[interception-tools]: https://gitlab.com/interception/linux/tools
