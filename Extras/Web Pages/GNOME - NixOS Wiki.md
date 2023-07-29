%%
up:: [[NixOS]]
tags:: #source/internet #note/reference #source/documentation 
%% 
%%
#log/study
Topic:: [[NixOS]]
Notes:: Learned about setting up gnome on my pi
Date:: 2023-07-29
Related::  [[Raspberry Pi]] [[GNOME]]
%%

# GNOME

From NixOS Wiki

[GNOME](https://gnome.org/) ([wikipedia:en:GNOME](https://en.wikipedia.org/wiki/en:GNOME "wikipedia:en:GNOME")) is available as a [module](/wiki/Module "Module") (and also especially as a module for `services.xserver.desktopManager`).

GNOME (/(ɡ)noʊm/) is a [desktop environment](/wiki/Desktop_environment "Desktop environment") that aims to be simple and easy to use. It is designed by The GNOME Project and is composed entirely of free and open-source software. GNOME is a part of the GNU Project.

## Installing GNOME

To use GNOME, add this to your configuration.nix:

```
services.xserver.enable = true;
services.xserver.displayManager.gdm.enable = true;
services.xserver.desktopManager.gnome.enable = true;
```

## Excluding some GNOME applications from the default install

Not all applications that come pre-installed with the GNOME desktop environment are desirable for everyone to have on their machines. There's a way to edit `configuration.nix` to exclude these kinds of packages, for example as follows:

```
environment.gnome.excludePackages = (with pkgs; [
  gnome-photos
  gnome-tour
]) ++ (with pkgs.gnome; [
  cheese # webcam tool
  gnome-music
  gnome-terminal
  gedit # text editor
  epiphany # web browser
  geary # email reader
  evince # document viewer
  gnome-characters
  totem # video player
  tali # poker game
  iagno # go game
  hitori # sudoku game
  atomix # puzzle game
]);
```

## Running GNOME programs outside of GNOME

While we are packaging GNOME platform-based applications to be largely [self-contained](https://nixos.org/nixpkgs/manual/#sec-language-gnome), they still depend, for one reason or another, on some global configuration. The `gnome.nix` module sets all the necessary options for you but if you are running customized set-up, you might need to replicate that yourself.

For instance, if you see the following error:

```
 dconf-WARNING **: failed to commit changes to dconf: GDBus.Error:org.freedesktop.DBus.Error.ServiceUnknown: The name ca.desrt.dconf was not provided by any .service files
```
you should enable dconf module:

```
programs.dconf.enable = true;
```
Many applications rely heavily on having an icon theme available, GNOME’s Adwaita is a good choice but most recent icon themes should work as well.


```
environment.systemPackages = [ gnome.adwaita-icon-theme ];
```
## Managing extensions

GNOME extensions are managed and configured by the program "Extension" that comes with GNOME.

Extensions to be installed system-wide by adding them to NixOS configuration in `environment.systemPackages` or per-user, or from the GNOME extensions website using a Web browser extension.

## Systray Icons

To get systray icons, install the related gnome shell extension


```
environment.systemPackages = with pkgs; [ gnomeExtensions.appindicator ];
```
And ensure gnome-settings-daemon udev rules are enabled :
```
services.udev.packages = with pkgs; [ gnome.gnome-settings-daemon ];
```
## Running ancient applications

Long ago, in the GNOME 2 era, applications used GConf service to store configuration. This has been deprecated for many years but some applications were abandoned before they managed to upgrade to a newer dconf system. If you are running such application and getting an error like:
```
GLib.GException: Failed to contact configuration server; the most common cause is a missing or misconfigured D-Bus session bus daemon. See http://projects.gnome.org/gconf/ for information
```
you need to add `gnome2.GConf` to the list of dbus packages in your `configuration.nix`:


```
services.dbus.packages = with pkgs; [ gnome2.GConf ];
```
After applying the update one also has restart their desktop session to refresh the user-specific dbus session.

## Troubleshoots

### Change user's profile picture

Currently there is no way to change the user's profile picture using Gnome Control Center [(see this issue)](https://github.com/NixOS/nixpkgs/issues/10025) and currently there is no plan to support it officially in NixOS. However, you can modify it by copying the profile picture that you want to the path **/home/$USER/.face** as a workaround, i.e.

```
mv /path/to/image.jpg ~/.face
```
### automatic login

If you have enabled [auto login](https://help.gnome.org/admin/system-admin-guide/stable/login-automatic.html.en) (with GNOME) with something like

```
grep autoLogin /etc/nixos/configuration.nix

services.xserver.displayManager.autoLogin.enable = true;
services.xserver.displayManager.autoLogin.user = "account";
```
than add the following (as a workaround for a current (2023)[[1]](#cite_note-1) problem)
```
nano /etc/nixos/configuration.nix

systemd.services."getty@tty1".enable = false;
systemd.services."autovt@tty1".enable = false;
```
