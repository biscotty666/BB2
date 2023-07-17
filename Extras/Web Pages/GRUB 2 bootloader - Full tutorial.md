up:: [[Linux]]
tags:: #note/reference #source/internet #note/tutorial

# GRUB 2 bootloader - Full tutorial

Updated: December 4, 2009; November 30, 2017

Dedoimedo definite GRUB 2.00 multi-boot tutorial [featured](http://www.linuxuser.co.uk/news/linux-user-developer-117-is-out-now/) in the 117th issue of the Linux User & Developer Magazine! You really should take a closer look.

GRUB 2 bootloader is in continuous development. Some information may change over time, as features are added or removed and fixes introduced. For latest updates, please take a look at [Updates](#updates) section below.

This tutorial focuses on GRUB version 2, the next generation of the popular bootloader. If you are looking for the original (legacy) GRUB tutorial, please take a look at [this](grub.html) article.

Welcome to the GRUB 2 bootloader tutorial! You must have read my GRUB legacy guide. In the last two and a half years, it alone has garnered some half a million views, proving to be quite popular and useful to computer users worldwide.

I want to recreate the same success with my GRUB 2 tutorial. My goal is to provide people running any flavor of UNIX-like operating systems or multi-booting their computers and using GRUB as their bootloader with a simple, no-nonsense, step-by-step, proven and working tutorial that should allow them to quickly, easily and painlessly control the boot sequence of their systems.

In this tutorial, you will learn how to work with GRUB 2, add and remove menu entries, customize titles and boot options, dual-boot and triple-boot operating systems, combine legacy GRUB and GRUB 2, and we will even see how Windows fits into this scheme. After that, we will learn how to recover from errors and mistakes. Follow me.

![Teaser](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-teaser.jpg)

  

## Table of Contents

  

1. [Warning](#mozTocId703454)
2. [GRUB 2 roadmap](#mozTocId433027)
3. [GRUB 2 introduction](#mozTocId547366)
    1. [New layout](#mozTocId920681)
    2. [Sample grub.cfg file](#mozTocId542243)
    3. [Sample /etc/grub.d/ directory](#mozTocId226706)
    4. [Sample /etc/default/grub](#mozTocId706228)
4. [Basic usage](#mozTocId398970)
    1. [How GRUB 2 works?](#mozTocId514088)
    2. [Add new GRUB script](#mozTocId584691)
    3. [Update GRUB](#mozTocId293325)
    4. [Change script boot order by changing numbers](#mozTocId463716)
    5. [Replacing default entries](#mozTocId205199)
    6. [OS Prober](#mozTocId835620)
    7. [Reinstall GRUB](#mozTocId232162)
    8. [Mini summary](#mozTocId105919)
5. [Real life multi-boot cases](#mozTocId928097)
    1. [Dual boot: two operating systems with GRUB 2](#mozTocId16468)
    2. [Dual boot: two operating systems with GRUB legacy & GRUB 2 mix](#mozTocId982259)
    3. [Dual boot: Windows 7 and Ubuntu](#mozTocId722095)
    4. [Upgrade from GRUB legacy to GRUB 2](#mozTocId113708)
6. [Additional test case scenarios](#mozTocId980832)
    1. [Triple boot](#mozTocId802943)
7. [Customize GRUB menu](#mozTocId315265)
    1. [Edit /etc/default/grub](#mozTocId624480)
    2. [Remove memtest from the menu](#mozTocId49740)
    3. [Change titles in menu entries](#mozTocId852131)
    4. [Change theme (boot image)](#mozTocId930378)
8. [GRUB 2 recovery](#mozTocId842078)
    1. [Recover from failed boots](#mozTocId905459)
9. [Small problems observed](#mozTocId872565)
    1. [Kernel crash dump mechanism](#mozTocId661841)
    2. [Command names](#mozTocId438865)
10. [GRUB legacy versus GRUB 2](#mozTocId452216)
    1. [GRUB legacy](#mozTocId374375)
    2. [GRUB 2](#mozTocId547491)
    3. [Verdict](#mozTocId397537)
11. [More reading (documents & links)](#mozTocId773498)
    1. [Official](#mozTocId914268)
    2. [Other useful GRUB 2 resources](#mozTocId181204)
12. [Conclusion](#mozTocId329294)
13. [Updates](#mozTocId668931)

  

## Warning

Warning! GRUB 2 is still mostly beta software. Although it already ships with Ubuntu flavors, it is not yet production quality per se. You can probably use it without any troubles, although there is a slight, remote yet possible chance of damage.

You need to be aware of this. Furthermore, whenever handling delicate tasks like the critical functions of the system, disk partitioning, boot sequence, imaging, etc, you should always be prepared for the worst. This means:

- You must have a solid, proven backup procedure for all your data.
- You must possess the tools and the knowledge to quickly recover from disasters. This includes being able to restore a previous system image, fix broken configurations, restore the bootloaders, and more.
- You must be confident in what you are doing.

Now that we know this, let us proceed cheerfully and safely. Just remember that GRUB 2 is still beta. Although the same can be claimed for Google Mail, which was beta for some six years or so, you must exercise caution. What's more, the contents and relevance of contents in this tutorial might yet change as GRUB 2 makes into the production, so stay tuned for any updates.

## GRUB 2 roadmap

This is something you should consider before trying GRUB 2. When will GRUB 2 become the de facto bootloader for UNIX-like operating systems? Currently, GRUB legacy is doing fine and will continue for many more years. Given the long-term support by companies like RedHat and Novell for their server distributions, GRUB legacy is going to remain the key player for at least 5-10 years.

On desktops, the adoption rate may be faster, but do not expect any miracles too soon. Nevertheless, it does not hurt to start exploring. Be aware that you may encounter some compatibility issues down the road, especially with more conservative distributions that do not embrace new technologies too quickly.

At the time being, GRUB 2 is only used by the Ubuntu family, which makes about a third to one half of the Linux desktop market. That makes this tutorial rather relevant, as about one in every two or three Linux home users will probably be interested in learning more about GRUB 2 and its uses. Let's now move on to the actual mechanics.

## GRUB 2 introduction

Before you dig in, I strongly advise you to read my [original](grub.html) GRUB tutorial. This will help you understand this article better. GRUB 2 introduces many new changes. GRUB 2 has better portability and modularity, supports non-ASCII characters, dynamic loading of modules, real memory management, and more. All these are pretty much irrelevant for most users. What you need to know are the changes in the configuration files and the way GRUB 2 operates.

### New layout

Old GRUB files were (and still are!) located under /boot/grub/, including the menu.lst file that was read during boot and which contents were displayed to the user in the form of the GRUB menu. GRUB 2 places its files in three core locations:

/boot/grub/grub.cfg - This is the main configuration file that replaces menu.lst. Unlike menu.lst, this file cannot be edited by hand! I strongly advise against trying to tamper with this file, using chattr command or anything of the sort. Let it be.

/etc/grub.d/ - This new directory contains GRUB scripts. These scripts are building blocks from which the grub.cfg file is built. When the relevant GRUB command is executed, the scripts are read in a certain sequence and grub.cfg is created.

/etc/default/grub - This file contains the GRUB menu settings that are read by the GRUB scripts and written into grub.cfg. It is the customization part of the GRUB, similar to the old menu.lst, except the actual boot entries.

This means that if you want to change the GRUB menu, you will have to edit existing scripts or create new ones, then update the menu. This is more similar to LILO than GRUB legacy, which allow editing the menu on the fly.

### Sample grub.cfg file

The file is located under /boot/grub/:

![Boot grub](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-boot-grub.jpg)

This is what the grub.cfg file looks like:

![grub.cfg](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-grub-cfg.jpg)

It is not really interesting, because it's just a shell script. Most people will probably not want to read its contents or be able to fully understand them. This file is still worth a quick look any time you update the GRUB menu, to make sure the correct entries are included as you expected. But only if you're comfortable with scripts!

### Sample /etc/grub.d/ directory

These are the contents of the directory on a fresh installation, in this case Kubuntu:

![Directory](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-dir-kubuntu.jpg)

Let's review the scripts:

00_header is the script that loads GRUB settings from /etc/default/grub, including timeout, default boot entry, and others. We will talk more about these soon.

05_debian_theme defines the background, colors and themes. The name of this script is definitely going to change to when other distributions adopt GRUB 2.

10_linux loads the menu entries for the installed distribution.

20_memtest86+ loads the memtest utility.

30_os-prober is the script that will scan the hard disks for other operating systems and add them to the boot menu.

40_custom is a template that you can use to create additional entries to be added to the boot menu.

Have you noticed the numbering in the script names? Well, this is somewhat similar to the order of Start/Kill scripts used in different runlevels. The numbering defines precedence. This means that 10_linux will be executed before 20_memtest86+ and therefore placed higher in the boot menu order.

The scripts are not very interesting. Like the grub.cfg file, they are not intended to be edited, save for 40_custom. You need to very careful when working with these scripts. We will soon see what they look like and learn how to use them.

### Sample /etc/default/grub

The file is located under /etc/default:

![Default dir](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-etc-default-dir.jpg)

This directory contains many other files so do not assume it is just used for GRUB. The grub file is a text file that is parsed by the 00_header script. You can make your changes here, if you want. We will talk about these later, in the Customization section.

![Grub](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-etc-default-grub.jpg)

Now we know what the core files are. Let's see how we use them.

## Basic usage

It's time to put our theoretical knowledge to some real action.

### How GRUB 2 works?

GRUB 2 works like this: /etc/default/grub contains customization; /etc/grub.d/ scripts contain GRUB menu information and operating system boot scripts. When the update-grub command is run, it reads the contents of the grub file and the grub.d scripts and creates the grub.cfg file.

That's all. To change the grub.cfg file, you need to edit the grub file or the scripts under grub.d. Scripts are meant to be executed. This means that they have the execute bit turned on. If you turn the execute bit off, they will not run.

This means that you can place as many files as you want into the grub.d directory, as long as they are not executable shell scripts that update-grub can read. If you want to use them, you will activate the executable bit, or vice versa, turn it off. Let's examine the scripts. For instance, 00_header and 05_debian_theme:

![00_header](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-00.jpg)

![05_debian_theme](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-05.jpg)

40_custom makes a little more sense, but it still does not tell us how we can customize the scripts. Don't worry, we will soon learn everything.

![40_custom](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-40.jpg)

### Add new GRUB script

To add a new boot option, you will have to follow a basic syntax:

Create a new file that has a XX_ prefix in the name, where XX is a sequence of numbers. If you want the new entry to be placed above others, use lower numbers, if you want it to be placed below others, use higher numbers.

For example, 11_something will be placed after the default entries by the operating system, whereas 08_something will be placed before the 10_linux entries. The next step is to write the actual content. Here's a sample:

#!/bin/sh -e  
echo "Some string"  
cat << EOF  
menuentry "Something" {  
set root=(hdX,Y)  
-- boot parameters --  
}  
EOF

Let's examine the file. It's a shell script, as declared in the first line.

echo "Some string" is a string that you will see when running update-grub. If you do not want to see the echo command printed, you can redirect it to standard error or /dev/null:

echo "Some string" > &2

Example: echo "Adding openSUSE 11.2"

cat << EOF defines the start of the actual boot entry.

menuentry "Something" is the name that will show in the menu. Example: Linux.

set root=(hdX,Y) - we're back to old school, setting the root device.

Critical! GRUB 2 uses PARTITION notation that starts with 1 and not 0 like GRUB legacy! This is terribly important to remember!

In other words, devices are still numbered from 0, but partitions start with 1. For example, this means that sda1 is now (hd0,1) and NOT (hd0,0) as before!

-- boot parameters -- will really change from one OS to another. In Linux, you may want to use something like:

linux /boot/vmlinuz  
initrd /boot/initrd.img

But in Windows, you would probably use:

chainloader (hdX,Y)+1

Therefore, a complete script example would look something like:

#!/bin/sh -e  
echo "Adding my custom Linux to GRUB 2"  
cat << EOF  
menuentry "My custom Linux" {  
set root=(hd0,5)  
linux /boot/vmlinuz  
initrd /boot/initrd.img  
}  
EOF

Or for Windows, something like:

#!/bin/sh -e  
echo "Adding Windows 8 to GRUB 2 menu"  
cat << EOF  
menuentry "Windows 8" {  
set root=(hd0,1)  
chainloader (hd0,1)+1  
}  
EOF

EOF ends the GRUB entry.

Now we have a file ready. But we need to make it executable.

chmod +x XX_new_os_script

GRUB 2 commands

One more thing I'd like to emphasize here is the data contained in the cat << EOF section. As we've seen just now, the cat command defines the start of the code in the script that will be added to the GRUB menu literally and NOT interpreted by the shell. In other words, anything that goes between cat << EOF and EOF are GRUB commands.

We have used a number of different commands in this script. Some of these you may have seen before in GRUB legacy, some may appear new and strange. Not to worry, there's a full list, including a comparison to GRUB legacy. Some of the commands have been replaced and others added. For more details, please check [GRUB 2 command list](http://www.gnu.org/software/grub/manual/grub.html#Command_002dline-and-menu-entry-commands).

### Update GRUB

The new script is in place, but the GRUB menu (grub.cfg) has not been updated yet. We need to run the update-grub command to make it happen.

update-grub

Here's an example from a dual-boot configuration, which we will examine more deeply later on. I've added two Kubuntu entries and one Ubuntu entry to the Ubuntu GRUB menu. I have done this in order to get rid of the default 10_linux, which is ugly and uses generic kernel names in the titles, plus it calls both Ubuntu and Kubuntu the same. But let's move slowly. Our first task is to add the new scripts. After we ascertain that they boot fine, we will get rid of the duplicates.

![New scripts](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-new-scripts.jpg)

![Updating](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-updating-grub.jpg)

After rebooting, we have this - I apologize for the uncomeliness:

![New scripts in place](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-menu-new-scripts.jpg)

Once we are using the new scripts boot properly, we can then chmod -x the 10_linux and get rid of the generic 2.6.31-14 entries, keeping our menu nice and dandy.

### Change script boot order by changing numbers

You can also change the numbers if you want. For instance, I changed the Kubuntu entries to 08 and 09, to make them boot first, before default entries and the custom Ubuntu script.

![Reorder](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-reorder.jpg)

### Replacing default entries

This is something you may want to do, just as we have shown above. In order to make sure your new scripts boot correctly, you can refer to a working example of grub.cfg to make sure you use the right linux and initrd lines. Here's an example of what I have:

![Template](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-grub-cfg-template.jpg)

And accordingly, I've created the new script:

![Copy](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-new-script-copied.jpg)

### OS Prober

OS Prober can also help you. It will find additional entries on your hard disks and add them to the menu. You can use the added information to create your own scripts. Again, refer to the grub.cfg file, os-prober section, for more data:

![OS prober](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-os-prober.jpg)

### Reinstall GRUB

GRUB 2 can be installed even while you are booted in the OS. You do not need a live environment for that. Just execute the grub-install command against the device or the partition you desire.

grub-install <target>

<target> can be /dev/hda, /dev/sdb, /dev/sdc4, and so forth. It is important that you pay attention to the output produced by the command. If you have external disks connected at the time you run the command, it will add these disks to the list of mapped devices. There should be no big harm in that, but if some entries are incorrect, remove them.

grub-install reports incorrect devices

Here's an example of that:

![Install](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-install.jpg)

You will need to open the /boot/grub/device.map file, delete the wrong entry, in our case the one pointing to /dev/sdb and rerun the grub-install command.

![Install again](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-install-again.jpg)

### Mini summary

That's it for now. You have the tools you need to start working. Still, to make things clearer, I've prepared a short summary that concludes the introduction and basic usage sections. Here we go. GRUB 2 has three main parts:

1. /etc/default/grub - the file containing GRUB 2 menu settings.
2. /etc/grub.d/ - the directory containing GRUB 2 menu creating scripts.
3. /boot/grub/grub.cfg - the GRUB 2 configuration file, not editable.

update-grub command reads the /etc/grub.d directory and looks for executable scripts inside it. The scripts are read, in the order of their numbering, and written into the grub.cfg file, along with the menu settings read from the /etc/default/grub file.

Boot entries come from several sources - the default that comes with the distribution, other operating systems probed on the connected disks and custom scripts written by the user, following a strict syntax. The scripts are written as shell (sh).

You can add/remove entries by simply chmod-ing the scripts; no need to delete them. GRUB 2 can be reinstalled anytime you want, even while booted in the OS. Good so far? Excellent. You see, it's rather simple. Now, let's see a few real-life cases. Afterwards, we will customize GRUB 2 and learn how to recover from serious errors and misconfigurations.

## Real life multi-boot cases

In this section, we will discuss several common multi-boot cases that the average user might encounter, including GRUB and GRUB 2 mix, Windows and Linux side by side, chainloading, and some others.

Some of the experiments were conducted on real physical systems, so some screenshots of boot menus come from actual photos, so they may look slightly twisted and less appealing that you would like, but this is in order to reassure you that working with GRUB 2 is as simple as any other administrative task. It just requires some patience and care.

### Dual boot: two operating systems with GRUB 2

This is probably the simplest dual-boot configuration. Both operating systems use GRUB 2, so they can easily interact with one another. The two systems are Ubuntu 9.10 and Kubuntu 9.10, both formatted with Ext4.

Ubuntu was installed first, with GRUB 2 installed to the MBR of /dev/sda. Kubuntu was installed second, again to MBR, overwriting the previous instance. However, the OS probe script found and added Ubuntu successfully. If you've read my [Kubuntu](kubuntu-9-10.html) tutorial, you will notice that Kubuntu entries are also labeled Ubuntu, which can be confusing if you have several, different Ubuntu entries on your machine. In this section, we will:

- Learn how to edit existing entries and make them more presentable.
- Add new entries and reorder existing ones.

This will give us the initial clues into how GRUB 2 works and what users have to do to make it work. After that, we will examine bootloading GRUB 2 ready systems from the legacy GRUB and vice versa. And then, we will add Windows, too.

We have seen snippets of this testcase earlier. We have our default entries, which do not look well and want them replaced. So we refer to grub.cfg file, examine the syntax used by 10_linux and we copy the relevant bits of data into our new scripts, called 08_9_10_kubuntu and 09_9_10_kubuntu_recovery. Just like we did earlier.

![Entry 1](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-new-script-copied.jpg)

![Recovery entry](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-new-script-recovery.jpg)

After that, we need to chmod +x the scripts and update the GRUB. Very simple.

![New update](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-new-update.jpg)

![New scripts](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-menu-new-scripts.jpg)

After rebooting and verifying that everything works, we can get rid of the 10_linux, having replaced its entries with the custom scripts. Job done, very easy!

### Dual boot: two operating systems with GRUB legacy & GRUB 2 mix

This is a very interesting case. Let's say you have two operating systems, like Ubuntu 9.10, which uses the new GRUB 2 and boots from Ext4, and a more classic distro like openSUSE 11.2, which does use Ext4, but still boots the old GRUB.

We will learn how to:

- Boot Ubuntu from the context of the openSUSE GRUB.
- Boot openSUSE from the context of the Ubuntu GRUB.
- Edit entries in both systems to make things work.

Decision: which one is default?

This is something you need to decide. You have two bootloader options and you need to choose the default one. If you're asking me, at the time being, you should use GRUB legacy as your default bootloader, because it is production quality and has known, established support channels. Running GRUB 2 will place you in a minority. Therefore, if you are going to use GRUB legacy for booting, then you should:

- Install GRUB into the MBR when installing the distribution that ships it, like openSUSE in our example. Pay attention to these details when installing the distro!
- Install GRUB 2 into the root partition where the distribution using it is installed, in this case Ubuntu. Pay attention to these details when installing the distro.

Otherwise, if you plan on using GRUB 2 as your default, you need to do things the other way around.

![Bootloader choice](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/ubuntu-9-10-bootloader.jpg)  

![Default selection](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/opensuse-11-2-bootloader-default.jpg)

![Bootloader options](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/opensuse-11-2-bootloader-options.jpg)

If you are not really sure, review the partition table.

![fdisk](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-fdisk.jpg)

In our example, Ubuntu was installed first, with root under /dev/sda6 and home under /dev/sda7. openSUSE was installed next, to /dev/sda1. Since we want to use the GRUB legacy bootloader, then:

- Ubuntu bootloader needs to be installed to /dev/sda6.
- openSUSE bootloader needs to be installed to MBR (/dev/sda).

Now that this is done, let's boot and see what happens. After you install openSUSE, you will notice that the lovely green GRUB menu contains only openSUSE entries. The reason is, GRUB legacy does not know how to handle the GRUB 2 layout directly and therefore cannot find and append entries to the menu. You will have to do this manually.

Boot into openSUSE and open the menu.lst file in a text editor. Back it up first! We do not really know yet how to work with GRUB 2 entries from within GRUB legacy. So we will assume that it's the same thing like Windows. We will chainload the other operating system, passing the command to the other bootloader. Will this work? I do not know, but let's try! We're using our GRUB experience and trying to adapt to the new situation.

![Chainload](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-add-chainload.png)

For those who cannot see what's in the image, we are adding the following entry to the menu.lst file:

title Ubuntu 9.10 Karmic Koala  
root (hd0,5)  
chainloader (hd0,5)+1

After booting, you will see the menu (once again, excuse the ugliness):

![GRUB menu](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-grub-menu-chainload.jpg)

If you try to boot the Karmic Koala entry, you will hit this unwholesome GRUB error 13:

![Unsupported format](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-unsupported-format.jpg)

This kind of error is common for Linux users trying to boot Windows and Mac partitions. For Windows, the solution is to try to remap the partitions, hide partitions or make them active, which usually solves the problem.

Mac-wise, the problem is similar to what we are facing here. GRUB legacy that comes with openSUSE does not handle well the Ubuntu partitions + GRUB 2 mechanism. We will need a different method to get things done. For more information, check [GRUB error messages](http://www.linuxselfhelp.com/gnu/grub/html_chapter/grub_13.html#SEC101).

Booting Ubuntu (with GRUB 2) from openSUSE the right way

Here's what you need to do to make things work. Go back to openSUSE and open the menu.lst file again. We need to make it look like this:

![GRUB correct entry](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-correct-entry.jpg)

For those who cannot see what's in the image, we are adding the following entry to the menu.lst file:

title Ubuntu 9.10 Karmic Koala  
root (hd0,5)  
kernel /boot/grub/core.img  
savedefault  
boot

You may be wondering what we did here. The answer is, core.img file is a small kernel image that allows you to boot the right drivers and properly identify and initialize the real kernel. You can think of this file as a sort of a transitional initrd.img. After rebooting, you will have the GRUB 2 menu. Select the desired entry and boot.

Once inside Ubuntu, you can do a little trick if you want, that is, rerun the update-grub script. It will now pick up the installed openSUSE as well and add it to the menu. You will then have a recursive GRUB menu that calls the other that calls the other. This is not necessary, but can be fun and exercises the power of knowledge and control.

![Mixed environment](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-mixed-env.png)

Another interesting case is [Mandriva 2010](mandriva-2010.html) and Ubuntu 9.10 dual boot. When installing Mandriva, you have the option to manually edit the GRUB bootloader menu even before the first boot. The truth is, many distributions let you configure the boot menu to some degree. If you know what you need, you can save time by setting up the menu entries during the installation. Here's what editing the GRUB menu following Mandriva installation looks like:

![Add Ubuntu](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/mandriva-2010-add-ubuntu.png)

Basically, we have done the exact same thing we did with openSUSE. For more details, please take a look at the review above.

### Dual boot: Windows 7 and Ubuntu

This will probably interest quite a few users. In general, it's the same as before. However, to spice things up a little, I have also added another element of interest here, that of the GRUB to GRUB 2 upgrade.

Test case: a machine dual booting Windows 7 and Ubuntu Jaunty, as shown in my new dual boot [guide](dual-boot-windows-7-ubuntu.html). Jaunty uses GRUB legacy, but it can be upgraded to GRUB 2, which is what we're going to do. So, we will first go through the upgrade procedure, paying attention to important details and then, we will make sure our Ubuntu and Windows installations boot fine.

### Upgrade from GRUB legacy to GRUB 2

The first thing is to install the GRUB 2 package:

sudo apt-get install grub2

Watch the terminal. You will soon have to answer a few prompts.

Chainload first

The first thing you will have to decide upon is whether to install GRUB2 into MBR right away or test first by installing to the root partition where Ubuntu is installed and chainloading from GRUB legacy. We have done this earlier, by calling on the core.img file, so we know how to do this. For the sake of elegance, we won't skip this step.

![Chainload first](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-upgrade-chainload-first.jpg)

![Chainload](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-upgrade-chainload.jpg)

The next step is to verify the command-line parameters. Most people won't need to make any change here.

![Options](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-upgrade-options.jpg)

GRUB 2 is now installed:

![Done](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-upgrade-done.jpg)

Before we reboot, make sure the grub.cfg has been built correctly and that it contains the right entries, pointing to the right devices and partitions. Use fdisk if you need to verify that everything is in order. Ubuntu entry:

![Linux entry](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-upgrade-linux-entry.jpg)

Windows entry:

![Windows entry](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-upgrade-windows-entry.jpg)

It's time to reboot and test. Your GRUB menu should be changed now and include a Chainload entry for GRUB 2 at the top of the menu.

![Chainload into GRUB 2](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-chainload.png)

Possible errors

You may encounter GRUB Error 11 or GRUB Error 15 when you try to boot into GRUB 2.

![Error](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-error.png)

For instance, GRUB Error 11 means the wrong root device is selected or that you're booting devices by ID rather than numbers, in which case you will have to change one of the strings to make it work.

To remedy the issue, you will have to highlight the Chainload entry, press e to edit, then change the root line to reflect your real selection. Don't forget you're working with GRUB legacy still, so partitions are numbered from 0. In the worst case, cycle through root (hdX,Y) until you nail the right one. Change the root entry, hit Enter, press b to boot.

Alternatively, if your GRUB uses strange, long string called device IDs rather than numbers, you will have to replace the string root with uuid and then you should be able to boot your kernel just fine. [HowtoForge](http://www.howtoforge.com/how-to-install-grub-2-on-ubuntu-9.04) has a great example, with screenshots. Eventually, you should see the GRUB 2 menu:

![GRUB 2 menu](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-dual-boot-in-grub2.png)

As you can see, we have Ubuntu entries and we have Vista loader (Windows 7) entries. All seems well. Now, boot both Ubuntu and Windows 7 to make sure everything works properly. Once you're satisfied, boot into Ubuntu and complete the migration process. You can now write GRUB 2 into MBR and make it the default bootloader.

sudo upgrade-from-grub-legacy

![Complete migration](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-complete-migration.jpg)

Pay attention to the device mapping and change accordingly. But that's it. We're done. We have now learned both how to upgrade GRUB, handle some boot errors and successfully boot a dual boot configuration including Windows and Linux.

## Additional test case scenarios

### Triple boot

From here on, it's damn easy. Use whatever you want. If you're running GRUB legacy as your bootloader, then you will use Windows entries like we did in the original article, plus an entry with /boot/grub/core.img for GRUB 2 entries.

And if you're using GRUB 2, then use OS Prober to find other operating systems. And if you don't like the titles, create custom entries based on the existing selection. The world is your banana, now.

## Customize GRUB menu

Let's say we want to edit the GRUB 2 menu a little, including the default selection, the timeout and other options.

### Edit /etc/default/grub

This file contains a few interesting options you may want to change, including the default selected entry, the default timeout and additional options.

![/etc/default/grub](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-etc-default-grub.jpg)

GRUB_DEFAULT=0 specifies the default entry. It counts from 0, like any geeky menu. Change to anything you like. If you set the entry to GRUB_DEFAULT=saved, it will boot the last selected option from the previous boot.

GRUB_TIMEOUT="10" specifies the default timeout. Change to anything you want. Very small values are not recommended. Setting to -1 will make GRUB wait indefinitely until you manually select an entry and hit Enter.

There are many other options, as you can see, I will not go through all of them. It's important that you remember that they exist and where they can be found. Do not blindly make changes. Consult the official documentation and always backup the file before tampering. One more thing that may interest you is the removal of recovery entries.

Remove recovery entries from the menu

The last line in the /etc/default/grub file allows you to disable the recovery mod menu entries. Simply uncomment the line and update GRUB:

GRUB_DISABLE_LINUX_RECOVERY="true"

### Remove memtest from the menu

If you do not want to have the memtest entries included, simply chmod -x the 20_memtest script. It will no longer be executable and won't be read into the grub.cfg file the next time you update GRUB.

sudo chmod -x 20_memtest86+

### Change titles in menu entries

Instead of using the replacement tweaks I've suggested earlier, by recreating the default Linux entries and removing (chmod-ing -x) the 10_linux script, you can edit the actual script to behave differently and report information in a more human format. This is some really geeky stuff, take at look at [Ubuntu install title tweaks](http://ubuntu-install.blogspot.com/2009/11/grub2-title-tweaks.html).

### Change theme (boot image)

You may want to use a beautiful image during boot rather than the boring blue one. No worries, this can be easily done, by editing the 05_debian_theme script.

The first thing is to find a number of images you want. Pay attention to image detail, as you want to achieve the right contrast between the entries and the background, so you can still read the menu. I will show you my own example - learn from it.

Next, you need a directory to store the images. By default, GRUB 2 takes images from /usr/share/images/desktop-base. This can be a good location - or if you want your own, create one. For instance, /usr/share/images/grub, just as shown on [Ubuntu blog](http://www.ubuntu-inside.me/2009/06/howto-change-grub2-grub-pc-splash-image.html).

Copy your images there, mind the extension. By default, GRUB 2 works with .png and .tga files. You can also use .jpg images if you want. The next step is to edit the 05_debian_theme script and change the relevant entry to point to your own:

![Add new image](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-theme-add-new-image.jpg)

What I did was comment out the original for loop that reads the image(s) and created my own. If you're not really comfortable doing this, then you should probably postpone any theme tweaking until a later date. My file is located: /usr/share/images/grub/dark.png. Once the file has been changed, run update-grub to update the grub.cfg file. If you've done everything correctly, you should see Found Debian background message in the terminal.

![Found new image](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-found-new-image.jpg)

Please note that the GRUB menu will not use background images unless these are enabled. To do that, you will have to change the line use_bg=false in theme script and change it to use_bg=true. Reboot and test:

![New theme](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-new-theme.jpg)

As you can see, my choice of background image was bad! You cannot see the text! Therefore, choose carefully what kind of background image you want to use. A theme with a few soft gradients seems like the best idea. For more details, take a look at the official Wiki documentation.

## GRUB 2 recovery

### Recover from failed boots

What happens if you ruin your GRUB 2? There must be a way to reinstall it and save the day? Well, as always, there's the easy way and the hard way.

Easy way: Super Grub Disk

[This](http://www.supergrubdisk.org/) great tool works with GRUB 2, so no worries, place it into the CD/DVD tray, boot and restore the mangled GRUB. You should keep an image handy and ready, just in case.

Hard way: Manual fix from live CD

You will need a live CD that ships with GRUB 2, like Ubuntu or Kubuntu. Boot into the live session, mount the hard disk and install GRUB 2 to the MBR. This is the sequence of commands you require (assuming disk = /dev/sda). You will need to mount the partition of your installed distribution (e.g. Ubuntu) containing the /boot directory. It may also be a separate partition on your system, depending on your setup. After it is mounted, you will have to rerun the grub-install command.

mount /dev/sda1 /mnt/  
grub-install --root-directory=/mnt /dev/sda

If this does not work, you will have to go through a much longer, more complicated procedure, as explained in the [official](https://wiki.ubuntu.com/Grub2) GRUB 2 Ubuntu Wiki documentation.

Use fdisk to locate the right root device, then mount it as before. For the sake of this section, let's assume that /dev/sda1 is a dedicated /boot partition and /dev/sda2 is the root partition of your distro.

sudo mount /dev/sda2 /mnt

If you have a separate /boot partition, you will have to mount it too:

sudo mount /dev/sda1 /mnt/boot

Next, remount the rest of your devices using --bind option. For more details about how the mount command works, please consult the [man](http://linux.die.net/man/8/mount) page.

sudo mount --bind /dev /mnt/dev

Effectively, you now have a root system under mount. You can now change the root from your real one (/) to /mnt. This is done using the chroot command.

sudo chroot /mnt

This means that command executed in this terminal will refer to /mnt/. Now, reconfigure the GRUB package:

dpkg-reconfigure grub-pc

You will have to tell GRUB which device to use. You can change the selection using Spacebar. The devices shown will reflect that in the device.map file, so if you only have one, there should be no issues. Once this step is done, you can exit from the chroot environment. This is done by pressing Ctrl + D keys. After that, unmount the devices, first the /dev, then the rest:

sudo umount /mnt/dev  
sudo umount /mnt

Now you can reboot. GRUB 2 should be in place.

## Small problems observed

This might be beta or just new features, but I must comment on a number of items I have observed, which could also help you troubleshoot problems in the future more easily.

### Kernel crash dump mechanism

GRUB 2 includes a section that checks if a kernel crash dump mechanism like [Kdump](kdump.html) is installed configured and appends a section to the kernel line. This is not a good idea, because the script does not check the offset where it can place the crash kernel. Furthermore, the memory allocation should be done by the admin and not arbitrarily, because we do not know how big or small the crash kernel should be.

![Kernel crash mechanism](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_new_2/grub2-kernel-crash.jpg)

### Command names

The two major commands, grub-install and update-grub do not align well. Either both should begin with a grub prefix or both should have a grub suffix.

## GRUB legacy versus GRUB 2

Now the big question, why one should you (not) use?

### GRUB legacy

It is older and no longer developed, but works great and has proven its worth many times over. Editing the GRUB menu is a very simple thing and require little skill on the behalf of the user, save for making sure the right syntax is followed.

### GRUB 2

GRUB 2 is beta software at the moment, although version 1.97 could easily become the official one. This makes it a less likely candidate for production systems. Additionally, GRUB 2 is more difficult to work with, because a) it requires shell skill, which is not something everyone can do b) changes to the GRUB menu are more difficult to implement and require three separate steps rather than one as before c) the changes are not automatic and have to be "compiled" into the menu every time, similar to what LILO used to do.

GRUB 2 is currently supported by a very small number of distributions, thus you are less likely to receive support and find answers online, including official and unofficial circles. What more, there's a chance you will encounter problems when using GRUB 2 in a mixed environment.

### Verdict

I have shown in both GRUB tutorials that even difficult tasks can be achieved relatively easily. It is possible to enjoy both GRUB legacy and GRUB 2, as my detailed examples clearly show. At the moment, though, you are probably better off with GRUB legacy, especially if you are a less knowledgeable user. If you're running Ubuntu, then you can stick with GRUB 2, as it comes as default, just make sure you follow my instructions for a happy and care-free grubbing.

## More reading (documents & links)

You are most advised to take a look at the following articles, reviews and howtos:

### Official

[GRUB 2 official site](http://www.gnu.org/software/grub/)

[GRUB 2 Manual](http://www.gnu.org/software/grub/manual/grub.html)

[GRUB 2 Ubuntu Wiki documentation](https://wiki.ubuntu.com/Grub2)

[GRUB 2 Command List](http://grub.enbug.org/CommandList)

### Other useful GRUB 2 resources

[GRUB bootloader - Full tutorial](grub.html) (my article)

[Herman's GRUB pages](http://members.iinet.net/%7Eherman546/p20.html) (excellent collection of howtos)

[GRUB error messages](http://www.linuxselfhelp.com/gnu/grub/html_chapter/grub_13.html#SEC101)

[How to change GRUB 2 theme](http://www.ubuntu-inside.me/2009/06/howto-change-grub2-grub-pc-splash-image.html)

[GRUB 2 title tweaks](http://ubuntu-install.blogspot.com/2009/11/grub2-title-tweaks.html) (advanced stuff)

[GRUB 2 on archlinux Wiki](http://wiki.archlinux.org/index.php/GRUB2) (advanced stuff)

[How to install GRUB 2 on Ubuntu 9.04](http://www.howtoforge.com/how-to-install-grub-2-on-ubuntu-9.04)

## Conclusion

That's it. You now have a step-by-step guide for installing, configuring and troubleshooting GRUB 2, including the overview of the layout and basic functions, numerous real-life examples like dual boot scenarios with GRUB legacy, GRUB 2 and Windows mixed together, as well as tools and instructions how to troubleshoot problems. This guide should be very handy for you, whether you're a beginner user, a recent Windows convert or a Linux veteran.

Compared to GRUB legacy, GRUB 2 is somewhat harder to use, as it requires familiarity and confidence working with shell, which is outside the realm of most users. Furthermore, it does not easily permit changes and the boot configuration has to be rebuilt every time one is introduced. The true power of GRUB 2 is yet to be revealed.

I hope you are going to like this tutorial as much as the original. I will update it periodically if certain core functions in GRUB change toward production release, as well as write any important tip or trick that comes up. Have fun and spread the word!

## Updates

All and any updates regarding GRUB 2 will be listed here, including new features, bug fixes and additional test cases, as well as any other useful material, tips and tricks you find and recommend. I suggest you occasionally check this section.

### December 2009, update:

Several question asked by my readers:

What if I use Ext3 filesystem? Can my GRUB boot partitions formatted with Ext4?

The answer is: maybe. If your distro supports Ext4, then yes, if it does not, probably not, because it won't be able to read anything from Ext4-formatted partitions. The easiest way to overcome filesystem compatibility issues is to use a dedicated /boot partition formatted with Ext3. This way, all modern Linux distros will support it.

I have two disks. Ubuntu Karmic is on the second disk. Do I have to use core.img file in my GRUB or can I chainload the usual way?

The answer is: if you have a bootloader installed to the root of the second disk, then you can probably use the traditional chainloading method. But if you're using a single bootloader, then you must resort to core.img file.

Why there's a difference between the 40_custom and your custom scripts?

The answer is: 40_custom script is already configured to be used within the context of other scripts when building the GRUB configuration file, at the end. Custom scripts that you write can go anywhere. Hence, the exec tail -n +3 $0 in the script that you need not use in your own scripts.

Will the custom scripts be updated if my other operating systems get kernel updates?

The answer is: Not automatically, and you'll have to rewrite them to point to relevant kernels. A way of solving this is to create a symbolic link to the latest kernel and the initrd image using vmlinuz and initrd generic names, respectively. This is what openSUSE does, allowing you to keep your GRUB menu static. The only change that needs to be done is to update the symbolic links following kernel updates, without touching the GRUB.

I can't get my custom backgrounds used in the GRUB menu anywhere!

See the use_bg section in the debian_theme script and make sure it's set to true.

### June 2010, update:

GRUB 2 is currently at version 1.98 and this is considered a stable release. There are some small changes in the overall functionality, but 99% of what you see above still applies.

GRUB 2 rescue CD

If you're not using Super Grub Disk (SGD) or have a live CD of a Linux distribution supporting GRUB 2 available, then you may want to consider creating a GRUB 2 rescue CD of your own. Another advantage of this procedure is that the rescue CD will contain your custom GRUB menu.

This is done by using the grub-mkrescue command, which has a slightly different usage syntax for [Lucid Lynx](ubuntu-lucid.html), which uses GRUB2-1.98-1ubuntu5 and [Karmic](ubuntu-9-10.html) and earlier, which use beta versions of the bootloader. Now, here's how to do it. Ubuntu 10.04:

grub-mkrescue --output=<name>.iso /boot/grub

You can use any name you want. Ubuntu 9.10 and earlier:

grub-mkrescue --overlay=/boot/grub <name>.iso

After creating the ISO image, you should [burn](linux-burn-image.html) it and then test it, preferably on another machine, to make sure the GRUB is booted from the CD and not the local disk. If you use a filename without a full path, the ISO image will be created in the current directory.

On the target (test) host, configured to boot from CD, the rescue CD should boot into GRUB commandline. To make sure that the CD works as expected and contains your GRUB menu, please run the following command:

configfile /grub.cfg

This should output your menu. Once satisfied with the change, keep the disc in standby for emergencies. Remember to create a new ISO any time you update the GRUB menu.

You should use the rescue CD to boot into desired operating system and then repair the GRUB using grub-install command, as we've seen earlier. The rescue CD will not work if you delete the operating system or partitions containing them. References:

[How to make your own GRUB2RESCUE CD-ROM](http://members.iinet.net/%7Eherman546/p20/GRUB2%20Bash%20Commands.html#GRUB2_CD-ROM)

There are also discussion threads available at Ubuntu forums and wilderssecurity.com, but they most revolve around the usage of these commands and the variations for different Ubuntu releases. Thanks to Ocky for this tip!

### December 2010, update:

A few more changes yet. Here's the brief overview and update.

Notation

Once again, the notation has changed. On MS-DOS type partitions, which represents the absolute majority of partition out there, GRUB2 in Maverick uses a new naming convention. Instead of the generic hd(X,Y), now you have hd(X,msdosY), which could be a little confusing. You must take this into account when creating complex multi-boot setups. For more details, please take a look at my [Maverick](ubuntu-maverick.html) review.

Additional reading

You're also welcome to take a look at the [official](http://ubuntuforums.org/showthread.php?t=1195275) Ubuntu forums thread on GRUB2.

### December 2011, update:

Several more changes. GRUB is officially at version 1.99 and comes with a few more changes. The important cosmetic fix is that additional, older versions of the kernel are all listed under a single directory-like entry, making the menu easier to read. Moreover, Ubuntu comes with a high-resolution menu and smaller font size. Now, let's examine several other things.

Booting Xen kernels in a multi-boot configuration governed by GRUB2

You have seen this example in my [Xen](xen-intro.html) intro tutorial. Specifically. Please note that if you're chainloading [openSUSE](opensuse-12-1-low-end.html) with GRUB2, then you will need to create a special entry for the Xen kernel, which might not be automatically added by the OS probe script. The entry looks different from standard boot stanzas.

multiboot /boot/xen.gz  
module /boot/vmlinuz-xen <options>  
module /boot/initrd-xen

Of course, adjust the exact paths to match your installations, devices, etc. Here's a screenshot of what it looks like on my test machine:

![GRUB 2 menu](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_years/2011_1/xen-grub.jpg)

Fedora 16 Verne support

[Fedora 16](fedora-16-kde.html) also comes with GRUB2. Some of the settings are a little different from Debian-based systems. In fact, you may find Fedora a little tricky to navigate after you've used GRUB2 on Ubuntu and family. Fedora GRUB2 support is still very early and rather buggy, with some of the functionality not yet implemented well.

How to update the GRUB menu

The update command is as follows:

grub2-mkconfig -o /boot/grub2/grub.cfg

This will recreate the grub.cfg file based on your scripts under /etc/grub.d.

![grub.d on Fedora](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_years/2011_2/grub2-fedora-16-grubd.png)

Edit header defaults (entry and timeout)

Changing the default entry and the timeout in Ubuntu is fairly trivial. This is not so in Fedora. Editing the 00_header file is more complicated. You must actually change the script functionality to get what you need.

The timeout is defined by make_timeout() function. The function accepts two input parameters, GRUB_TIMEOUT and GRUB_TIMEOUT_BUTTON, which are declared at the beginning of the 00_header script.

![Header file](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_years/2011_2/grub2-fedora-16-header-file.png)

However, the variables will get different values based on a variety of environmental settings and configurations. If you find it too hard tracking down the execution, you may want to brute-force the change.

In the make_timeout() function, you can edit the declaration set timeout=${2} to a static value. The script will then ignore the input parameter and use whatever you choose. For example, you may want to increase the timeout from 5 seconds to 20 seconds.

![make_timeout function](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_years/2011_2/grub2-fedora-16-make-timeout.png)

In that case, change the script as follows:

#set timeout=${2}  
set timeout=20

Make necessary backups first! And there's no need to delete anything, just comment out the lines you do not want to use. This way, you will be able to revert back to the original state if needed.

After editing the file, you must recreate the grub.cfg file. Once the file is recreated, you can verify that your change has been configured properly by grepping the timeout value in the configuration file.

![Changed timeout](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_years/2011_2/grub2-fedora-16-changed-timeout.png)

And here we are booting; notice the timeout counter.

![Booting](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_years/2011_2/grub2-fedora-16-changed-timeout-boot.png)

More reading

You may also want to consult the following online pages:

[GRUB2 - FedoraProject.org](http://fedoraproject.org/wiki/Grub2)

[GRUB2 features - FedoraProject.org](http://fedoraproject.org/wiki/Features/Grub2)

If you feel a separate Fedora & GRUB2 tutorial is needed, we can arrange that too.

### June 2012, update:

GRUB 2.00 has been [officially](http://lists.gnu.org/archive/html/grub-devel/2012-06/msg00093.html) released. It comes with all kinds of goodies. We will be seeing GRUB 2 in its major release version coming to various distributions later this year. For the end user, the changes should be transparent. Just use this tutorial and have massive fun.

You might also want to look at my GRUB article in the 117th [issue](http://www.linuxuser.co.uk/news/linux-user-developer-117-is-out-now) of the Linux User & Developer magazine released in October 2012. It's basically a multi-boot tutorial with focus on the GRUB 2.00 release. Enjoy.

### January 2014, update:

A small update on how to update the GRUB configuration file on openSUSE. This is very similar to the exercise we have conducted on Fedora above. The grub.cfg file [needs](opensuse-pimp.html) to be created using the grub2-mkconfig command, with the output file flag.

grub2-mkconfig -o /boot/grub2/grub.cfg

You can then create test files and compare before committing changes.

### May 2014, update:

You might also be interested to [learn](dual-boot-windows-8-ubuntu.html) about an Asus Ultrabook setup that includes UEFI, Secure Boot, GPT partition table rather than MS-DOS, and a dual-boot configuration of Windows 8.1 and Ubuntu. The GRUB2 configuration is entirely transparent, because of the built-in support in the Ubuntu image. But as a use case, it is quite interesting, especially given the rather complex default layout of the laptop in question.

![Dual-boot setup complete](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers_years/2014_1/dual-boot-windows-8-ubuntu-done.jpg)

### April 2015, GRUB2 & EFI recovery:

If you need to recover GRUB on systems with UEFI and GPT, then I have a complete separate [guide](grub2-efi-recovery.html) for this. The procedure is a bit long and it warrants its own article, that's why I'm splitting from this already very long and detailed howto.

![List of partitions](https://www.dedoimedo.com/computers/grub-2.html#mozTocId547366../images/computers-years/2015-1/grub-efi-recovery-list.png)

### June 2015, GRUB2 & UEFI:

Furthermore, a reader named Phil has some additional interesting pointers. Namely, the location of the grub.cfg file is different on systems that use UEFI. You can locate the correct path on your system using the find command:

find /boot -name 'grub.cfg'

For instance, you may get /boot/efi/EFI/centos/grub.cfg. Therefore, if you need to update the configuration file, and you want to do it explicitly, without using the GRUB update command, you will need to specify the correct file as the output:

grub2-mkconfig -o /boot/efi/EFI/<os>/grub.cfg 

### February 2017, GRUB2 & UEFI errors:

I have published two additional tutorials that help address several new boot problem scenarios, including a [corrupt EFI partition](grub2-efi-corrupt-part-recovery.html) after an installation of a Linux distribution, and not being able to boot other operating systems in a multi-boot setup governed by a Red Hat based distro bootloader on UEFI systems due to [missing EFI modules](grub2-fedora-command-not-found.html) with linuxefi and initrdefi commands.

error: can't find command 'linux'  
error: can't find command 'initrd'

More to come ... P.S. If you find this article useful, please [support](../support-dedoimedo.html) Dedoimedo.

Cheers for now, stay tuned for updates.

![facebook sharing button](https://platform-cdn.sharethis.com/img/facebook.svg)

![twitter sharing button](https://platform-cdn.sharethis.com/img/twitter.svg)

![reddit sharing button](https://platform-cdn.sharethis.com/img/reddit.svg)

![email sharing button](https://platform-cdn.sharethis.com/img/email.svg)

You may also like:

- [About](/about.html)
- [Contact](/contact_me.html)
- [FAQ](/faq.html)
- [Terms](/terms_of_use.html)

- [Advertise](/advertise-new.html)
- [Support](/support-dedoimedo.html)
- [Subscribe](/rss_feed.xml)

- [Home](/index.html)
- [Top](#)

Valid [HTML](https://validator.w3.org/check?uri=referer) [CSS](https://jigsaw.w3.org/css-validator/validator?uri=https://www.dedoimedo.com) [RSS](https://validator.w3.org/feed/check.cgi?url=https://www.dedoimedo.com/rss_feed.xml)

[CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/) @ Dedoimedo 2006-2023

|   |   |
|---|---|
|||