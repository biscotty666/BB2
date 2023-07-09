# Alacritty

- [Page](/title/Alacritty "View the content page [alt-shift-c]")
- [Discussion](/title/Talk:Alacritty "Discussion about the content page [alt-shift-t]")

 English

- [Read](/title/Alacritty)
- [View source](/index.php?title=Alacritty&action=edit "This page is protected.
    You can view its source [alt-shift-e]")
- [View history](/index.php?title=Alacritty&action=history "Past revisions of this page [alt-shift-h]")

 More

- [Read](/title/Alacritty)
- [View source](/index.php?title=Alacritty&action=edit)
- [View history](/index.php?title=Alacritty&action=history)

From ArchWiki

[Alacritty](https://github.com/alacritty/alacritty) is a simple, GPU-accelerated terminal emulator written in [Rust](/title/Rust "Rust"). It supports scrollback, 24-bit colors ([w:Color depth#True color (24-bit)](https://en.wikipedia.org/wiki/Color_depth#True_color_.2824-bit.29 "w:Color depth"), copy/paste, clicking on URLs, and custom key bindings.

## Installation

[Install](/title/Install "Install") the [alacritty](https://archlinux.org/packages/?name=alacritty) package or [alacritty-git](https://aur.archlinux.org/packages/alacritty-git/)AUR for the development version.

## Configuration

_Alacritty_ searches for a configuration file at the following places in this order:

- `$XDG_CONFIG_HOME/alacritty/alacritty.yml`
- `$XDG_CONFIG_HOME/alacritty.yml`
- `$HOME/.config/alacritty/alacritty.yml`
- `$HOME/.alacritty.yml`

Copy the example configuration file at `/usr/share/doc/alacritty/example/alacritty.yml` to one of those places and uncomment the settings you want to change. Most settings take effect as soon as you save the file.

### Colors

See [the upstream wiki](https://github.com/alacritty/alacritty/wiki/Color-schemes) for a list of available color schemes. If your preferred color scheme is on the list, paste the provided code into your configuration file.

### Font

If you do not want to use your system’s default font, you can specify a different font by changing the following lines:

font:
  normal:
    family: monospace
    style: Regular

  bold:
    family: monospace
    style: Bold

  italic:
    family: monospace
    style: Italic

  bold_italic:
    family: monospace
    style: Bold Italic

  size: 11

Substitute `monospace` with a font name from the output of

$ fc-list : family style

Note that some fonts do not provide an `Italic` style but an `Oblique` style instead.

## Tips and tricks

### Spawn new instance in same directory

Add the following lines to your configuration file to spawn a new instance of _Alacritty_ in the current working directory by pressing `Ctrl+Shift+Enter`:

key_bindings:
  - { key: Return,   mods: Control|Shift, action: SpawnNewInstance }

### Vi mode and copy/paste

The vi mode allows moving around Alacritty's viewport and scrollback using the keyboard. By default, you can toggle it using `Ctrl+Shift+Space`. To copy, you can either use a mouse to select and press `Ctrl+Shift+c`, or enter Vi mode, start a selection using `v`, move around with `hjkl` like in vim, and copy the selection with `y`. To paste, press `Ctrl+Shift+v`. To copy/paste to/from X clipboard, you can use a mouse selection to copy and a middle mouse click to paste.

### Regex hints

Regex hints can be used to find text in the visible part of the terminal and pipe it to other applications. When activated, the matched texts are highlighted with "hints" according to the configured alphabet and presented to the user for selection via keyboard.

By default, Alacritty provides hinting URLs via the `Ctrl+Shift+U` shortcut and opens them with [xdg-open](/title/Xdg-open "Xdg-open"). See the `hints:` section of `alacritty.yml` for configuration details.

## Troubleshooting

### Mouse not working properly in Vim

Add `set ttymouse=sgr` and `set mouse=a` to your `.vimrc` or switch to [Neovim](/title/Neovim "Neovim"). Also see [this issue](https://github.com/alacritty/alacritty/issues/803).

### Transparent border in dwm

With [dwm](/title/Dwm "Dwm"), alacritty's borders become transparent. Adding this line to `drw.c` in the [dwm](/title/Dwm "Dwm") source directory and recompiling fixes the issue:

if (!XftColorAllocName(...))
    die("error, cannot allocate color '%s'", clrname); /* Find this line */
**dest->pixel |= 0xff << 24;** /* Add this line */

### Terminal functionality unavailable in remote shells

When connecting to a remote system from an Alacritty terminal, for instance over [SSH](/title/SSH "SSH"), it can occur that the system does not have an entry for Alacritty in its terminfo database (`/usr/share/terminfo/a/alacritty*`). Therefore, all interactive terminal functionality does not work. This can be fixed by copying the terminfo for Alacritty to the remote server, as described in [termite#Terminal issues with SSH](/title/Termite#Terminal_issues_with_SSH "Termite").

On the local host, using Alacritty:

$ infocmp > alacritty.terminfo  # export Alacritty's Terminfo
$ scp alacritty.terminfo user@remote-host:~/  # or any other method to copy to the remote host

On the remote host, in the directory where you copied `alacritty.terminfo`:

$ tic -x alacritty.terminfo  # import Terminfo for current user
$ rm alacritty.terminfo  # optional: remove Terminfo file

Here is a one-liner version of the process above:

$ infocmp | ssh "$user@$host" 'tic -x /dev/stdin'

**Note:** After this, you will need to start a new SSH session to have the remote shell load the new Terminfo.

Alternatively, one can set the value of `TERM` in the configuration to `xterm-256color` instead of the default `alacritty`:

env:
  TERM: xterm-256color

### No title bar on Wayland GNOME

When using Wayland GNOME, the title bar is empty and has strange icons. See [https://github.com/alacritty/alacritty/issues/4739](https://github.com/alacritty/alacritty/issues/4739) for details.

One workaround is to launch Alacritty with [Xwayland](/title/Xwayland "Xwayland") instead of native Wayland by setting an empty `WAYLAND_DISPLAY` [environment variable](/title/Environment_variable "Environment variable").

### Different font size on multiple monitors

By default, Alacritty attempts to scale fonts to the appropriate point size on each monitor based on the `Device pixel ratio`. On some setups with multiple displays, this behavior can result in vastly different physical sizes [[1]](https://github.com/alacritty/alacritty/issues/3465), [[2]](https://github.com/alacritty/alacritty/issues/1339).

To view the existing device pixel ratio values for each monitor, run `alacritty -v`, move the child window to each monitor, and pay attention to the reported `Device pixel ratio` in the parent window.

Forcing a constant device pixel ratio using the `WINIT_X11_SCALE_FACTOR` environment variable may be sufficient to fix the different font size issue:

$ WINIT_X11_SCALE_FACTOR=1.66 alacritty

Retrieved from "[https://wiki.archlinux.org/index.php?title=Alacritty&oldid=782536](https://wiki.archlinux.org/index.php?title=Alacritty&oldid=782536)"

[Category](/title/Special:Categories "Special:Categories"):

- [Terminal emulators](/title/Category:Terminal_emulators "Category:Terminal emulators")

- This page was last edited on 5 July 2023, at 07:03.
- Content is available under [GNU Free Documentation License 1.3 or later](https://www.gnu.org/copyleft/fdl.html) unless otherwise noted.

- [Privacy policy](https://terms.archlinux.org/docs/privacy-policy/)
- [About ArchWiki](/title/ArchWiki:About)
- [Disclaimers](/title/ArchWiki:General_disclaimer)