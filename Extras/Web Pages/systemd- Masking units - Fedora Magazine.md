up::
tags:: #note/fleeting 
X:: 

## systemd- Masking units - Fedora Magazine



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

# systemd: Masking units

Posted by [Ashutosh Sudhakar Bhakare](https://fedoramagazine.org/author/ashutoshbhakare/ "Posts by Ashutosh Sudhakar Bhakare") on [November 18, 2015](https://fedoramagazine.org/systemd-masking-units/)

![](https://fedoramagazine.org/wp-content/uploads/2015/11/systemd-maskingunits-945x400.jpg)

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

Welcome back to our continuing series on systemd. In [an earlier article](https://fedoramagazine.org/what-is-an-init-system/), we discussed simple commands to manage the services on your [Fedora](https://getfedora.org) system. Here we are going to discuss a subtlety of systemd that makes it very powerful: masking. Before we get to masking, though, let’s review other levels of “off” for a service.

## The first two levels of “off”

You may know these common _systemctl_ commands:

systemctl start httpd.service
systemctl stop httpd.service

These commands take effect immediately to start or stop the web server (httpd) based on its unit file. You can think of _stop_ as the **first level of “off”** for a unit in systemd.

You might also recall these common commands:

systemctl enable httpd.service
systemctl disable httpd.service

These commands do not take effect immediately. They ensure the service will be started (or not) after the next system boot. You can think of _disable_ as the **second level of “off”** for systemd units.

You probably can tell where we’re going with this. Masking is a third level, less often used but quite powerful. But before we look at masking, we need to understand why it exists: dependencies.

## Unit dependencies

It’s important to remember, though, that systemd is more powerful than the legacy _init_ startup system. The old _init_ system would start or stop services in the order determined by their file names. These files were usually links placed in a directory named after the _runlevel_ in which they should operate. (If you need to refresh your memory on runlevels, refer to [this earlier systemd article](https://fedoramagazine.org/systemd-converting-sysvinit-scripts/).) For instance, consider this old initscript link:

/etc/rc3.d/S40myservice -> /etc/init.d/myservice

This link exists in the _/etc/rc3.d/_ folder. The link determined what happened when the system entered runlevel 3, or multi-user mode. The _S_ in the name means the script will be run in start mode. A _K_ instead would indicate the script would be stopped (killed). The number _40_ is an indicator of ordering. Lower numbered scripts run before this one, and higher numbered scripts run later. The actual script itself lived in the _/etc/init.d/_ folder.

Compared to systemd’s capabilities, this seems a bit primitive! Recall that systemd can understand dependencies between units. This means systemd can tell when one unit must be started to allow another to run.

Furthermore, systemd uses this information to start a required service, **even if disabled,** to satisfy dependencies. As an example, let’s look at some the dependencies of _httpd.service:_

$ systemctl list-dependencies httpd.service
httpd.service
 ● ├─-.mount
 ● ├─system.slice
 ● ├─tmp.mount
 ● ├─var.mount
 ● └─basic.target
 ●   └─-.mount
 ●   ├─alsa-restore.service
...

This graph shows us all the units on which the web server unit depends. Notice the graph is recursive, meaning it also shows dependencies of dependencies.

Let’s look at the effect of disabling a dependency. Assume that _httpd.service_ is enabled in systemd. Now we disable the system.slice service with the following command:

systemctl disable system.slice

By the way, disabling this unit isn’t a great idea. It could theoretically affect every service that systemd starts! As it happens, though, it doesn’t matter if we do. Because _httpd.service_ depends on _system.slice_,  disabling has no effect. systemd understands the requirements of _httpd.service_ and therefore it starts _system.slice_ regardless.

Now you can hopefully start to understand why masking exists. It allows the administrator to force systemd to do as instructed, even if it’s illogical.

## Masking: the third level

You may recall that systemd uses several locations to store unit information. If not, feel free to refer to [our previous article on unit files](https://fedoramagazine.org/systemd-getting-a-grip-on-units/) to refresh your memory. To record unit masking, systemd uses the local system configuration files in _/etc/systemd/._ It writes a link file pointing to _/dev/null_, the famous “nothing” file in UNIX and Linux. So for instance, masking _httpd.service_ would result in this link:

$ sudo systemctl mask httpd.service
Created symlink from /etc/systemd/system/httpd.service to /dev/null.

Note this is equivalent to running this command:

$ sudo ln -s /dev/null /etc/systemd/system/httpd.service

Now attempt to start the web server manually:

systemctl start httpd.service

You’ll see the following error message:

Failed to start httpd.service: Unit httpd.service is masked

This is the **third level of “off”** in systemd. If you boot with a unit masked, it will not run even to satisfy dependencies. Masking is powerful for this reason. But like other powerful commands, you should use it carefully. If you mask an important unit (like the aforementioned _system.slice_), you could stop your system from booting normally. To unmask the unit, use this command:

systemctl unmask httpd.service

Hopefully you can now appreciate even more the power of systemd units and dependencies.For some additional explanation of masking, refer to [this post](http://0pointer.de/blog/projects/three-levels-of-off) from the “systemd for administrators” blog series. See you soon for our next systemd installment!

### Like this:

Like Loading...

[For System Administrators](https://fedoramagazine.org/category/for-system-administrators/) [Using Software](https://fedoramagazine.org/category/software/)

[](https://fedoramagazine.org/author/ashutoshbhakare/)![](https://secure.gravatar.com/avatar/48ddc0f22e556d3bb3d454cfd118fb75?s=96&d=retro&r=g) 

#### [Ashutosh Sudhakar Bhakare](https://fedoramagazine.org/author/ashutoshbhakare/)

### 2 Comments

1. ![](https://secure.gravatar.com/avatar/9edf7084016e1ef425e7c1dfd814793d?s=160&d=retro&r=g)
    
    #### Aditya Konarde
    
    Nice and informative article.. Particularly helped clear my confusion between enable and start. Keep the good work going.
    
    [November 20, 2015](https://fedoramagazine.org/systemd-masking-units/#comment-452724 "November 20, 2015 at 11:02")
    
2. ![](https://secure.gravatar.com/avatar/59ac79945ef8c21b4a59d223a011515c?s=160&d=retro&r=g)
    
    #### Westerj
    
    Thanks for shedding a little light on the parallelisms and distinctions between Systemd and Traditional Init. This will help admins navigate the changes if the need arises.
    
    [November 24, 2015](https://fedoramagazine.org/systemd-masking-units/#comment-453479 "November 24, 2015 at 23:12")
    

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

![](https://pixel.wp.com/g.gif?v=ext&blog=58617187&post=10411&tz=0&srv=fedoramagazine.org&j=1%3A12.3&host=fedoramagazine.org&ref=&fcp=0&rand=0.24017676299606405)