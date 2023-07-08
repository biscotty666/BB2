up::
tags:: #note/fleeting 
X:: 

## What is an init system- - Fedora Magazine



---

### References
[![](https://fedoramagazine.org/wp-content/themes/fedoramagazine-1.15/images/fmag-ribbon.png)](https://fedoramagazine.org/)

- [fosstodon](https://fosstodon.org/@fedora)
- [Meta](https://www.facebook.com/TheFedoraProject)
- [Instagram](https://www.instagram.com/thefedoraproject)
- [Twitter](https://twitter.com/fedora)
- [YouTube](https://www.youtube.com/fedoraproject/videos)
- [Chat](https://chat.fedoraproject.org/)
- [Discourse](https://discussion.fedoraproject.org/)
- [RSS Feed](https://fedoramagazine.org/feed/ "						")

# What is an init system?

Posted by [Ryan Lerch](https://fedoramagazine.org/author/ryanlerch/ "Posts by Ryan Lerch") on [October 21, 2015](https://fedoramagazine.org/what-is-an-init-system/)

![](https://fedoramagazine.org/wp-content/uploads/2015/09/systemdpart1-945x400.png)

in this series

[![](https://fedoramagazine.org/wp-content/uploads/2015/09/systemdpart1.png)

### 1. What is an init system?

](https://fedoramagazine.org/what-is-an-init-system/)[![](https://fedoramagazine.org/wp-content/uploads/2015/10/systemdpart2.jpg)

### 2. systemd unit file basics

](https://fedoramagazine.org/systemd-getting-a-grip-on-units/)[![](https://fedoramagazine.org/wp-content/uploads/2015/10/systemd-part3.jpg)

### 3. systemd: Converting sysvinit scripts

](https://fedoramagazine.org/systemd-converting-sysvinit-scripts/)[![](https://fedoramagazine.org/wp-content/uploads/2015/11/systemd-part4.jpg)

### 4. systemd: Using the journal

](https://fedoramagazine.org/systemd-using-journal/)[![](https://fedoramagazine.org/wp-content/uploads/2015/11/systemd-maskingunits.jpg)

### 5. systemd: Masking units

](https://fedoramagazine.org/systemd-masking-units/)[![](https://fedoramagazine.org/wp-content/uploads/2015/10/systemd-deps.jpg)

### 6. systemd: Unit dependencies and order

](https://fedoramagazine.org/systemd-unit-dependencies-and-order/)[![](https://fedoramagazine.org/wp-content/uploads/2015/12/systemd-part7.jpg)

### 7. systemd: Template unit files

](https://fedoramagazine.org/systemd-template-unit-files/)[![](https://fedoramagazine.org/wp-content/uploads/2021/06/schedule_with_systemd_timer-400x200.jpg)

### 8. Systemd Timers for Scheduling Tasks

](https://fedoramagazine.org/systemd-timers-for-scheduling-tasks/)

Back in Fedora 15, Fedora introduced a piece of core software called systemd. **systemd** is a collection of tools for a range of different tasks. Its primary purpose is initializing, managing and tracking system services and daemons in Fedora, both during startup and while the system is running. In previous versions of Fedora, these jobs were handled by SysVinit (System V init, or just init).

To understand the primary purpose of systemd — initializing the Fedora operating system — first we need to understand what an init system actually does.

### Booting and initializing

Your computer’s startup or boot process begins with the BIOS (Basic Input / Output System) software on the motherboard. After completing hardware initialization and checks, the BIOS starts up the bootloader (GRUB in Fedora’s case). The bootloader accesses the Master Boot Record (MBR) on your storage device, usually a hard drive. It uses the data there to locate and start the Linux kernel.

The init system is the first process started on Fedora after the kernel starts. In fact, the init system always gets the Process ID (PID) of 1 on a system. This process is always executed by the Linux kernel after the early stages of bootup are completed by the BIOS and bootloader (GRUB).

## Daemons and other processes

The init system then must start all the necessary daemons, background processes and services required to make the operating system work. Examples of such processes include starting up a boot screen, various parts of the networking stack, and hardware in your machine.

Often these processes continue to run in the background after they start, and are called _daemons._ These daemons manage many parts of your system, such as logging information, watching for devices you insert or remove, and managing user login.

To see all the processes and daemons that the systemd init system is managing on your Fedora system, use the command

systemctl

in the terminal to list them.

After initialization, the init system stays running, for as long as your system is running, and it manages the processes already started, and allows others to be started, stopped and restarted while the OS is running.

## Controlling with systemd

The init system can also be used to start other services and daemons too. For example, let’s say you want your machine to start a webserver on boot. You can tell systemd to start the httpd daemon during boot:

sudo systemctl enable httpd

If you want the webserver to start right away, that’s easy as well:

sudo systemctl start httpd

Once you have started the service, you can check the status of the service with

systemctl status httpd

These are only simple examples of how to control specific services with systemd. Managing your Fedora system with systemd is not difficult, though. The Fedora community wiki [has a handy systemd page](https://fedoraproject.org/wiki/Systemd) to help you quickly learn the commands most often used with systemd. There’s also [a SysVinit to systemd cheatsheet](https://fedoraproject.org/wiki/SysVinit_to_Systemd_Cheatsheet) to help you translate, in case you’re used to the old SysVinit commands already.

## Introducing the systemd series

This is the beginning of a series of articles delving into systemd usage. The series will assume that you’ve read [the systemd overview page](https://fedoraproject.org/wiki/Systemd). Once you’ve done that, this series will show you other interesting ways to use systemd, and explain how it works. Get ready for a fun ride as you learn more about managing Fedora!

### Like this:

Like Loading...

[Fedora Project community](https://fedoramagazine.org/category/fedora-project/)

[](https://fedoramagazine.org/author/ryanlerch/)![](https://secure.gravatar.com/avatar/70316775017b196ded95244e23b8a094?s=96&d=retro&r=g) 

#### [Ryan Lerch](https://fedoramagazine.org/author/ryanlerch/)

Ryan is a designer that works on stuff for Fedora. He uses Fedora Workstation as his primary desktop, along with the best tools from the Libre Graphics world, notably, the vector graphics editor, Inkscape.

### 5 Comments

1. ![](https://secure.gravatar.com/avatar/a74e6a00f51d1ccbc27ee935d91d3648?s=160&d=retro&r=g)
    
    #### Eduardo Silva
    
    There’s a typo in the last two commands, should be
    
    httpd
    
    not
    
    http
    
    [October 21, 2015](https://fedoramagazine.org/what-is-an-init-system/#comment-443425 "October 21, 2015 at 12:22")
    
    - ![](https://secure.gravatar.com/avatar/70316775017b196ded95244e23b8a094?s=160&d=retro&r=g)[
        
        ](https://fedoramagazine.org/what-is-an-init-system/#comment-443606 "Comment by post author")
        
        #### [Ryan Lerch](http://ryanlerch.id.fedoraproject.org/)
        
        Thanks Eduardo!
        
        I have fixed it up now
        
        thanks again
        
        [October 21, 2015](https://fedoramagazine.org/what-is-an-init-system/#comment-443606 "October 21, 2015 at 20:22")
        
        - ![](https://secure.gravatar.com/avatar/73aedd050f561a866edd45e60b251717?s=160&d=retro&r=g)
            
            #### [Bubba](http://countryfiedlinux.tk)
            
            You look like Mick Foley.
            
            [October 21, 2015](https://fedoramagazine.org/what-is-an-init-system/#comment-443637 "October 21, 2015 at 22:05")
            
2. ![](https://secure.gravatar.com/avatar/84ef80657172ec698cf35bf86fbe7bc7?s=160&d=retro&r=g)
    
    #### Jose Gonzalez
    
    I thought BIOS looked at MRB for bootloader then the bootloader(GRUB) looked at it’s config for the kernel.
    
    [October 23, 2015](https://fedoramagazine.org/what-is-an-init-system/#comment-444618 "October 23, 2015 at 13:07")
    
3. ![](https://secure.gravatar.com/avatar/618246a54dc318c8b1f00f528c80d5ce?s=160&d=retro&r=g)
    
    #### mate
    
    BIOS looks in to MBR and loads a small program which knows where GRUB is located then this small program loads GRUB and eventually GRUB loads a kernel.
    
    [October 31, 2015](https://fedoramagazine.org/what-is-an-init-system/#comment-448789 "October 31, 2015 at 18:46")
    

Comments are Closed

![](https://fedoramagazine.org/wp-content/uploads/2023/04/f38-300x127.jpg)

[Fedora Linux 38 is available now](https://getfedora.org/). Read the [release announcement](https://fedoramagazine.org/announcing-fedora-38/) for all the details.

### Subscribe to Fedora Magazine via Email

Email Address 

    Subscribe

Join 9,569 other subscribers

### Contribute to the Magazine

Fedora Magazine is looking for contributors!

- [Propose a new article](https://docs.fedoraproject.org/en-US/fedora-magazine/writing-a-pitch/)
- [Become a writer](https://docs.fedoraproject.org/en-US/fedora-magazine/contributing/)
- [Become an editor](https://docs.fedoraproject.org/en-US/fedora-magazine/editorial/)

The opinions expressed on this website are those of each author, not of the author's employer or of Red Hat. Fedora Magazine aspires to publish all content under a Creative Commons license but may not be able to do so in all cases. You are responsible for ensuring that you have the necessary permission to reuse any work on this site. The Fedora logo is a trademark of Red Hat, Inc. [Terms and Conditions](/terms-and-conditions)

%d bloggers like this:

![](https://pixel.wp.com/g.gif?v=ext&blog=58617187&post=10202&tz=0&srv=fedoramagazine.org&j=1%3A12.3&host=fedoramagazine.org&ref=&fcp=0&rand=0.9895509346513485)