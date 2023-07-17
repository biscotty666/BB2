
# Manage Packages on Fedora Silverblue with Toolbox, rpm-ostree & Flatpak

By

[Josphat Mutai](https://computingforgeeks.com/author/mutai-josphat/)


I recently made a switch from Fedora Workstation to Fedora Silverblue. I can say the transformation was worth all the time and effort. _Fedora Silverblue_ is an immutable desktop operating system derived from Fedora Workstation. It gives you the look and feel of a regular desktop operating system, but with extreme stability and reliability.

```
$ neofetch
          /:-------------:\          jkmutai@localhost.localdomain 
       :-------------------::        ----------------------------- 
     :-----------/shhOHbmp---:\      OS: Fedora 31.20191219.0 (Workstation Edition) x86_64 
   /-----------omMMMNNNMMD  ---:     Host: 20LTS1RE0Y ThinkPad L480 
  :-----------sMMMMNMNMP.    ---:    Kernel: 5.3.16-300.fc31.x86_64 
 :-----------:MMMdP-------    ---\   Uptime: 2 hours, 5 mins 
,------------:MMMd--------    ---:   Packages: 1629 (rpm), 7 (flatpak) 
:------------:MMMd-------    .---:   Shell: zsh 5.7.1 
:----    oNMMMMMMMMMNho     .----:   Resolution: 1368x771 
:--     .+shhhMMMmhhy++   .------/   WM: sway 
:-    -------:MMMd--------------:    Theme: Adwaita [GTK2/3] 
:-   --------/MMMd-------------;     Icons: Adwaita [GTK2/3] 
:-    ------/hMMMy------------:      Terminal: termite 
:-- :dMNdhhdNMMNo------------;       Terminal Font: Monospace 9 
:---:sdNMMMMNds:------------:        CPU: Intel i5-8350U (8) @ 3.600GHz 
:------:://:-------------::          GPU: Intel UHD Graphics 620 
:---------------------://            Memory: 3920MiB / 15529MiB 

                                                             
                                                             

```

Fedora Silverblue aims to be an excellent platform for developers, Super SysAdmins and users doing container-focused workflows – Working with Toolbox, Podman, buildah, cri-o and Kubernetes. Silverblue is immutable, meaning the operating system on disk is exactly the same from one machine to the next, and it never changes as it is used.

## Software Management in Fedora Silverblue

Fedora Silverblue provide its users with different options for managing software applications as compared with other package-based Linux distributions.

These options are:

- **Using Toolbox**: This is used primarily to install CLI applications & development and debugging tools.
- **Using Flatpak:** For GUI applications, the primary installation way is through [Flathub](https://flathub.org/home) app store.
- **Package layering**: The _rpm-ostree tool_ is a hybrid image/package system with atomic upgrades and package layering.

## 1. Manage Software Packages on Fedora Silverblue with Toolbox

Silverblue comes with the _toolbox_ utility, which uses containers to provide an environment where development tools and libraries can be installed and used. The utility allows you to run multiple containers of Fedora Server (Without GUI) and install packages you want for Development purposes.

Toolbox provides a familiar package-based environment in which tools and libraries can be installed with the standard Fedora package manager – **dnf.**

### Advantages of using Toolbox

- **It keeps the host OS clean and stable** – You don’t have to install hundreds of packages on your host system which you don’t always use.
- **Isolate software dependencies**: Containers are a good way to isolate and organise the dependencies needed for different projects.
- **Best place to experiment** – If you want to play with new software, or unstable package releases, toolbox is the right place for this. You can always clean the environment when done without breaking your host system.
- **Keeps you happy** – You’ll always smile knowing your host system will never break as a result of buggy package being installed.

Toolbox uses the following technologies under the hood:

- [Buildah](https://buildah.io/)
- [OCI container images](https://www.opencontainers.org/)
- [Podman](https://podman.io/)

### Installing Toolbox

Fedora Silverblue ships with toolbox utility pre-installed. If for any reason the command line tool is not available, install it with the command:

```
$ rpm-ostree install toolbox
```

Since the package is layered, a reboot is required to boot to the image with toolbox. Toolbox can also be installed in Fedora Workstation by running:

```
sudo dnf install toolbox
```

### Using toolbox on Fedora Silverblue

- Once toolbox is installed, you can create your new container with the command:

```
$ toolbox create
```

You can provide a name for the container to be created, as well as specific Fedora release.

```
$ toolbox create --container myfed --release f35
Created container: myfed
Enter with: toolbox enter --container myfed
```

This will create a container called myfed from Fedora 31 base image. For Fedora 30, use **f30.**

- To List existing toolbox containers and images, use the command:

```
$ toolbox list
IMAGE ID      IMAGE NAME                                    CREATED
6387bd6f2368  registry.fedoraproject.org/fedora-toolbox:35  2 weeks ago

CONTAINER ID  CONTAINER NAME  CREATED        STATUS   IMAGE NAME
e4f812e69750  myfed           3 minutes ago  running  registry.fedoraproject.org/fedora-toolbox:35
```

- To enter a toolbox container for interactive use:

```
$ toolbox enter -c myfed
We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.


Welcome to the Toolbox; a container where you can install and run
all your tools.

 - Use DNF in the usual manner to install command line tools.
 - To create a new tools container, run 'toolbox create'.

For more information, see the documentation.

⬢[jmutai@toolbox ~]$ 
```

From the terminal you can use dnf to manage software packages like you’ll do in a Fedora Workstation machine.

```
sudo dnf -y install vim bash-completion curl wget 
```

- To remove a toolbox container, use:

```
### Remove all toolbox containers ###
$ toolbox rm -a

### Remove a toolbox container named myfed ###
$ toolbox rm myfed

### Force removal of running toolbox container ###
$ toolbox rm -f myfed
```

- To removes all existing podman toolbox containers, images and configurations:

```
$ toolbox reset
```

## 2. Manage Software Packages on Fedora Silverblue with Flatpak

Flatpak is the primary way that apps can be installed on Silverblue. The main source of Flatpak apps is [Flathub](https://flathub.org/home) – A large repository of Flatpak apps that can be installed.

### Using Flatpak

A setup of Flathub on Fedora Silverblue can be done easily by opening the [Flathub setup page for Fedora](https://flatpak.org/setup/Fedora/) and clicking the **“Flathub repository file**” button to download the Flathub configuration.

[![setup flatpak fedora](https://computingforgeeks.com/manage-packages-on-fedora-silverblue-linux/data:image/svg+xml,%3Csvg%20xmlns=%22http://www.w3.org/2000/svg%22%20width=%221024%22%20height=%22350%22%3E%3C/svg%3E "Manage Packages on Fedora Silverblue with Toolbox, rpm-ostree & Flatpak 1")](data:image/svg+xml,%3Csvg%20xmlns=%22http://www.w3.org/2000/svg%22%20width=%221024%22%20height=%22350%22%3E%3C/svg%3E)

Terminal users can also add the Flathub repository manually by running:

```
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

### Using Flatpak on Fedora

- To list the remotes that you have configured on your system, run:

```
$ flatpak remotes
Name    Options
fedora  system,oci
flathub system
```

- To search for Applications on repository, use the **search** command. For example:

```
$ flatpak search chromium
Name                 Description                                                Application ID                    Version       Branch      Remotes
Chromium B.S.U.      Fast paced, arcade-style, top-scrolling space shooter      net.sourceforge.chromium-bsu      0.9.16.1      stable      flathub

$ flatpak search skype
Name           Description                                                  Application ID                Version           Branch        Remotes
Skype          Call and message skype users, with video chat support        com.skype.Client              8.55.0.141        stable        flathub
Franz          Messenger for the desktop                                    com.meetfranz.Franz           5.4.1             stable        flathub
Discord        Chat client                                                  com.discordapp.Discord        0.0.9             stable        flathub
```

Each search result includes the **application ID** and the **remote** that the application is in.

- To install an application, run:

```
$ flatpak install flathub <APP-ID>
Example:
$ flatpak install com.skype.Client -y
......
 1. [✓] org.freedesktop.Platform                        x86_64            19.08             flathub            178.2 MB / 238.0 MB
 2. [✓] org.freedesktop.Platform.GL.default             x86_64            19.08             flathub             90.9 MB / 90.9 MB
 3. [✓] org.freedesktop.Platform.Locale                 x86_64            19.08             flathub             16.7 kB / 318.2 MB
 4. [✓] org.freedesktop.Platform.VAAPI.Intel            x86_64            19.08             flathub              8.7 MB / 8.7 MB
 5. [✓] org.freedesktop.Platform.openh264               x86_64            19.08             flathub            594.2 kB / 593.4 kB
 6. [—] com.skype.Client                                x86_64            stable            flathub             80.9 MB / 80.8 MB

Installing 6/6… ████████████████████ 100%  2.6 MB/s  00:00
```

- To list installed applications, run:

```
$ flatpak list --app

Name                                    Application ID                       Version            Branch         Origin          Installation
Skype                                   com.skype.Client                     8.55.0.141         stable         flathub         system
Chromium B.S.U.                         net.sourceforge.chromium-bsu         0.9.16.1           stable         flathub         system
GNU Image Manipulation Program          org.gimp.GIMP                        2.10.14            stable         fedora          system
File Roller                             org.gnome.FileRoller                                    stable         fedora          system
```

- To run installed application, use:

```
$ flatpak run com.skype.Client
```

- To update all your installed applications and runtimes to the latest version, run:

```
$ flatpak update
```

- To remove an application, run:

```
$ flatpak uninstall com.skype.Client
```

For more details, check [flatpak CLI reference](http://docs.flatpak.org/en/latest/using-flatpak.html) page.

## 3. Manage Packages on Fedora Silverblue with rpm-ostree

Most (but not all) RPM packages provided by Fedora can be installed on Silverblue using this method. This works by modifying your Silverblue installation to extend the packages from which Silverblue is composed.

Package layering creates a new “_deployment_“, or bootable filesystem root, and the system must be rebooted after a package has been layered. This preserves rollback and the transactional model.

- First, generate rpm repo metadata:

```
rpm-ostree refresh-md 
```

- A package can be installed on Silverblue using:

```
rpm-ostree install <package name>
```

Example:

```
$ rpm-ostree install vim
$ for i in neofetch zsh feh sway; do
  rpm-ostree install $i;
done
```

- You can as well replace a package with a different version using `rpm-ostree override` command:

```
rpm-ostree override replace <path to package>
```

- To uninstall a package, run:

```
$ rpm-ostree uninstall flameshot          
Checking out tree 675ab14... done
Resolving dependencies... done
Checking out packages... done
Running pre scripts... done
Running post scripts... done
Running posttrans scripts... done
Writing rpmdb... done
Writing OSTree commit... done
Staging deployment... done
Freed: 242.4 MB (pkgcache branches: 2)
Removed:
  flameshot-0.6.0-4.fc31.x86_64
  qt5-qtsvg-5.12.5-1.fc31.x86_64
Run "systemctl reboot" to start a reboot
```

## Performing Upgrades & Rollback on Fedora Silverblue

The standard behavior is to automatically download the updates and install them, but as a user, you can manually perform system updates.

```
$ rpm-ostree upgrade
```

Alternatively, you can check for available updates without downloading them, run:

```
$ rpm-ostree upgrade --check
```

### Performing Rollbacks

Silverblue keeps a record of the previous OS version, which can be switched to instead of the latest version.

There are two ways to roll back to the previous version:

1. **Temporary rollbacks**: Just reboot your system and select the _previous version_ from the boot menu to perform a temporarily roll back.
2. **Permanant rollback**: To switch back permanently to a previous deployment, use the command:

```
$ rpm-ostree rollback
```

That’s the basic ways of managing software packages on Fedora Silverblue. Other guides of interest are:

- [How To run Docker Containers using Podman and Libpod](https://computingforgeeks.com/using-podman-and-libpod-to-run-docker-containers/)
- [Setup Docker Container Registry with Podman & Let’s Encrypt SSL](https://computingforgeeks.com/create-docker-container-registry-with-podman-letsencrypt/)
- [How To Publish Docker Image to Docker Hub with Podman](https://computingforgeeks.com/how-to-publish-docker-image-to-docker-hub-with-podman/)
- [Uploading CentOS 8 Cloud Image to OpenStack Glance](https://computingforgeeks.com/uploading-centos-8-cloud-image-to-openstack-glance/)

up:: [[Silverblue]]
tags:: #note/reference #source/internet #source/article 