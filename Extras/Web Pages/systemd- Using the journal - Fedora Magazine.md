[![](https://fedoramagazine.org/wp-content/themes/fedoramagazine-1.15/images/fmag-ribbon.png)](https://fedoramagazine.org/)

up::
tags:: #note/fleeting 
X:: 

## systemd- Using the journal - Fedora Magazine



---

### References

## Basic journal use

The _journalctl_ command lets you interact with the journal. By default, _journalctl_ shows you the entire journal content:

journalctl

If you’re not new to Linux, you’ll recall that older systems use _[syslog](https://en.wikipedia.org/wiki/Syslog)_ to record log data. You’ll notice the output of the journal looks very much like _syslog_ output. Here’s an example from my host. Don’t worry if you don’t know what any of this means. These are messages from booting the system recently.

-- Logs begin at Fri 2015-11-06 16:55:01 EST, end at Tue 2015-11-10 17:01:23 EST. --
Nov 06 16:55:01 scarlett systemd-journal[200]: Runtime journal is using 8.0M (max allowed 297.9M, trying to leave 446.9M free of 2.9G available → current limit 297.9M).
Nov 06 16:55:01 scarlett systemd-journal[200]: Runtime journal is using 8.0M (max allowed 297.9M, trying to leave 446.9M free of 2.9G available → current limit 297.9M).
Nov 06 16:55:01 scarlett kernel: microcode: CPU0 microcode updated early to revision 0x19, date = 2013-06-21
Nov 06 16:55:01 scarlett kernel: Initializing cgroup subsys cpuset
Nov 06 16:55:01 scarlett kernel: Initializing cgroup subsys cpu
Nov 06 16:55:01 scarlett kernel: Initializing cgroup subsys cpuacct
Nov 06 16:55:01 scarlett kernel: Linux version 4.2.5-300.fc23.x86_64 (mockbuild@bkernel02.phx2.fedoraproject.org) (gcc version 5.1.1 20150618 (Red Hat 5.1.1-4) (GCC) ) #1 SMP Tue Oct 27 04:29:56 UTC 2015
Nov 06 16:55:01 scarlett kernel: Command line: BOOT_IMAGE=/vmlinuz-4.2.5-300.fc23.x86_64 root=/dev/mapper/fedora_scarlett-root ro rd.lvm.lv=fedora_scarlett/root rhgb quiet LANG=en_US.UTF-8
Nov 06 16:55:01 scarlett kernel: x86/fpu: Legacy x87 FPU detected.
Nov 06 16:55:01 scarlett kernel: x86/fpu: Using 'lazy' FPU context switches.
Nov 06 16:55:01 scarlett kernel: e820: BIOS-provided physical RAM map:
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x0000000000000000-0x000000000009ebff] usable
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x000000000009ec00-0x000000000009ffff] reserved
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x00000000000e4000-0x00000000000fffff] reserved
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x0000000000100000-0x00000000bf75ffff] usable
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x00000000bf760000-0x00000000bf76dfff] ACPI data
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x00000000bf76e000-0x00000000bf7a7fff] ACPI NVS
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x00000000bf7a8000-0x00000000bf7dffff] reserved
Nov 06 16:55:01 scarlett kernel: BIOS-e820: [mem 0x00000000bf7eb800-0x00000000bfffffff] reserved

The journal uses a pager by default, so you can use arrow and PgUp/PgDn keys to move around. Try it out, and notice that some lines are highlighted. Error messages, for instance, appear in red.

Since the journal is persistent, the journal could cover months of system logs. You might not want to page through all that output. If you’re looking for data from the most recent system boot, for example, you can use _-b_ to show just that information:

journalctl -b

But for many people this also is a lot to look through. This is especially true if you’re troubleshooting a specific service problem. Let’s say you’re trying to identify a problem with [NetworkManager](https://wiki.gnome.org/Projects/NetworkManager). You can filter the journal by systemd unit using the _-u_ switch:

journalctl -b -u NetworkManager

But what if your system has been up for weeks or months? That’s not uncommon with a Linux operating system like Fedora, after all. Fortunately you can also filter by time:

journalctl -b -u NetworkManager --since='yesterday'

Even more specifically, you can find errors by filtering on priority with _-p_. These priorities are the same as used in the old syslog system, such as _err_ for errors, or _warning_ for warnings.

journalctl -b -u NetworkManager --since='yesterday' -p err

## Journal metadata

But there’s a lot more to the journal than meets the eye. Each entry in the journal includes a large set of metadata.

If you use the _-o_ switch, you can switch the output format for the journal. By default, the journal uses the _short_ format, which mimics _syslog_. However, the _verbose_ setting will show you metadata for each journal entry:

$ journalctl -b -u NetworkManager --since='yesterday' -o verbose
Sun 2015-11-08 00:25:43.491639 EST [s=fdcd66beb0fc4bb1b46acabbb548fd69;i=e264b;b=a8ff73d157c541abbfaca338ecddccba;m=7192b31e50;t=52400b547291a;x=4a57fe2915e9a6c3]
 _MACHINE_ID=e72f725410ac48dfb979ead85d8ff44f
 _HOSTNAME=localhost
 _SYSTEMD_SLICE=system.slice
 _TRANSPORT=syslog
 _UID=0
 _GID=0
 SYSLOG_FACILITY=3
 PRIORITY=6
 SYSLOG_IDENTIFIER=dhclient
 _COMM=dhclient
 _EXE=/usr/sbin/dhclient
 _CAP_EFFECTIVE=203402
 _SYSTEMD_CGROUP=/system.slice/NetworkManager.service
 _SYSTEMD_UNIT=NetworkManager.service
 _SELINUX_CONTEXT=system_u:system_r:dhcpc_t:s0
 _CMDLINE=/sbin/dhclient -d -q -sf /usr/libexec/nm-dhcp-helper -pf /var/run/dhclient-wlp2s0.pid -lf /var/lib/NetworkManager/dhclient-d989668a-f8d3-4729-a2b0-d02b244e0161-wlp2s0.lease -cf /
 _BOOT_ID=a8ff73d157c541abbfaca338ecddccba
 SYSLOG_PID=2027
 _PID=2027
 MESSAGE=DHCPREQUEST on wlp2s0 to 192.168.1.254 port 67 (xid=0x6c674a2a)
 _SOURCE_REALTIME_TIMESTAMP=1446960343491639

## Using fields

You can filter the journal on any of these fields as well. Take for example the useful __BOOT_ID_ field. This field lets systemd identify the boot session to which a log entry belongs. As we already saw, there’s already a _journalctl -b_ option to show only log entries from the most recent boot.

But what about previous times before the system was rebooted? You can tell the journal to show you all recorded boot sessions:

$ journalctl --list-boots
-2 d30ee3a4f9ac4104aee3940d844e41fa Wed 2015-09-30 14:42:41 EDT—Wed 2015-09-30 14:58:03 EDT
-1 1c677fd72e82413bb68fe95f10524aef Fri 2015-10-23 11:02:15 EDT—Mon 2015-11-02 08:55:44 EST
 0 a8ff73d157c541abbfaca338ecddccba Mon 2015-11-02 08:55:51 EST—Sun 2015-11-08 21:01:01 EST

As you can see, the most recent boot session ID matches that in the journal entry above (starting with _a8ff73d1…_). This makes sense, because we requested the journal only show entries since the most recent boot. But now we can specifically request messages from the previous boot session.

And by the way, you can use the _Tab_ key to autocomplete the boot ID, to avoid an error in typing. Not only that, but you can autocomplete the field name too, For instance, type _journalctl _B_**[Tab]** and the field completes. Then you could type _1c_**[Tab]** using our example above, and the value completes:

journalctl _BOOT_ID=1c677fd72e82413bb68fe95f10524aef -u NetworkManager

Another useful field is __COMM_. You can use this to filter by a specific executable that may have resulted in an error. For instance, the NetworkManager service launches the _dhclient_ executable to get a network address for your Fedora system. If your system is not getting an address, you might want to see just the journal messages from dhclient:

journalctl _COMM=dhclient --since='2 hours ago'

If you want to know the available values for any field, you can use the _-F_ switch. Notice how this command, for instance, shows you similar information to _–list-boots:_

journalctl -F _BOOT_ID

For instance, you could use this command to find out the names of all systemd units the system has ever run:

journalctl -F _SYSTEMD_UNIT

## Logs made simple

Perhaps you’re not interested in using the journal through a terminal. Fortunately, there’s a useful application called _Logs_ you can get for your Fedora Workstation, or other edition. Install it via the Software application. It gives you an easy, point and click interface for looking at systemd log messges.

You may have seen this application earlier. In fact, we reported about it in Fedora Magazine [in this post](https://fedoramagazine.org/simply-view-system-logs-in-fedora-21-workstation/) for Fedora 21. You’ll be happy to know it is now lightning-fast, and loads messages on demand.

[![gnome-logs](https://fedoramagazine.org/wp-content/uploads/2015/11/gnome-logs-1024x646.png)](https://fedoramagazine.org/wp-content/uploads/2015/11/gnome-logs.png)

The log messages are separated by what generated the message. To choose a different category, click it on the left side of the app. You can also choose a specific boot session to examine logs. Click on the date and time header control to select the session you prefer. You can also search for specific issues using the search tool. Finally, to display details on any log message, click the message in the list.

Hopefully this article has shown you some useful and interesting ways to use the journal. For additional helpful information, refer to [this blog entry](http://0pointer.de/blog/projects/journalctl.html) in the original “systemd for administrators” series. Happy logging!

### Like this:

Like Loading...

[For System Administrators](https://fedoramagazine.org/category/for-system-administrators/) [Using Software](https://fedoramagazine.org/category/software/)

[](https://fedoramagazine.org/author/pfrields/)![](https://secure.gravatar.com/avatar/691b5bb2f0df8d2c07ade695df949319?s=96&d=retro&r=g) 

#### [Paul W. Frields](https://fedoramagazine.org/author/pfrields/)

Paul W. Frields has been a Linux user and enthusiast since 1997, and joined the Fedora Project in 2003, shortly after launch. He was a founding member of the Fedora Project Board, and has worked on docsc, websites, advocacy, toolchain, and package maintenance. He joined Red Hat as Fedora Project Leader from February 2008 to July 2010, and remains with Red Hat as an engineering manager. He currently lives with his wife and two children in Virginia where he also runs a recording studio (5thdom.com).

### 13 Comments

1. ![](https://secure.gravatar.com/avatar/e13d0de2f87a4fc7da78ca0d70604804?s=160&d=retro&r=g)
    
    #### Mohammadhzp
    
    Excellent, Thank you
    
    [November 12, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451236 "November 12, 2015 at 05:00")
    
2. ![](https://secure.gravatar.com/avatar/46fd9ef1b3ea70ee6f2b2cdffc52fb78?s=160&d=retro&r=g)
    
    #### archuser
    
    How do i clear these logs
    
    [November 12, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451253 "November 12, 2015 at 08:01")
    
3. ![](https://secure.gravatar.com/avatar/cdb830afbb6fac57218dc45f937a06e3?s=160&d=retro&r=g)
    
    #### Joao Rodrigues
    
    To clear the logs, you can use the options –vacuum-size= or –vacuum-time=
    
    For example:  
    journalctl –vacuum-size=1M
    
    journalctl –vacuum-time=1day
    
    You can also query how much disk space your logs are using with the option –disk-usage
    
    [November 12, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451291 "November 12, 2015 at 12:46")
    
4. ![](https://secure.gravatar.com/avatar/904a0a16c2c11905dc0dd34c5d35beb6?s=160&d=retro&r=g)
    
    #### msm
    
    Excellent.
    
    [November 12, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451293 "November 12, 2015 at 12:52")
    
5. ![](https://secure.gravatar.com/avatar/769084f2da2ebbc1e483af3f0e72adaa?s=160&d=retro&r=g)
    
    #### AC
    
    How do I examine log files, for instance those on a mounted drive from another system, not the logs of the system I’m using?
    
    [November 12, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451310 "November 12, 2015 at 15:10")
    
    - ![](https://secure.gravatar.com/avatar/691b5bb2f0df8d2c07ade695df949319?s=160&d=retro&r=g)[
        
        ](https://fedoramagazine.org/systemd-using-journal/#comment-451466 "Comment by post author")
        
        #### [Paul W. Frields](http://pfrields.id.fedoraproject.org/)
        
        @AC: Point journalctl at the directory with the mounted drive’s logs using the _-D_ option, or you can point it at the root of the mounted system using _–root=/mount/dir_ option.
        
        [November 13, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451466 "November 13, 2015 at 08:34")
        
6. ![](https://secure.gravatar.com/avatar/bc89293a8977ced248888c45830c6e9a?s=160&d=retro&r=g)
    
    #### cestlaz
    
    nice series Paul, thanks
    
    @archuser: check the Arch WIki at  
    [https://wiki.archlinux.org/index.php/Systemd#Journal_size_limit](https://wiki.archlinux.org/index.php/Systemd#Journal_size_limit)  
    I use a journal size limit, but you can also check under clean journal files manually
    
    [November 12, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451312 "November 12, 2015 at 15:29")
    
7. ![](https://secure.gravatar.com/avatar/fbb9cdae0cf161365b6cb0e005a46b35?s=160&d=retro&r=g)
    
    #### max
    
    Thanks!  
    journald keeps log data in binary format yeah?  
    How can I read the logs of an external / mounted vm image , say for auditing purposes?  
    Can I point systemctl to read log data from elsewhere: i.e. not the current running system?
    
    [November 12, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451326 "November 12, 2015 at 16:30")
    
    - ![](https://secure.gravatar.com/avatar/fbb9cdae0cf161365b6cb0e005a46b35?s=160&d=retro&r=g)
        
        #### max
        
        Sorry, I meant journalctl, not systemctl
        
        [November 13, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451425 "November 13, 2015 at 04:13")
        
    - ![](https://secure.gravatar.com/avatar/5562a0cab161344bc5df193614faab91?s=160&d=retro&r=g)
        
        #### foobit
        
        You can try journalctl –directory=/path/to/external/logs
        
        [November 13, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451534 "November 13, 2015 at 17:21")
        
8. ![](https://secure.gravatar.com/avatar/ac8b307c7e3ac6b6fc765b7afed8b79e?s=160&d=retro&r=g)
    
    #### marc
    
    Under the section “Using Fields” you don’t show the command to show recorded boot sessions. The command is ‘journalctl –list-boots’, which you mention later on, but you may want to make it more clear. Otherwise very helpful article, thanks!
    
    [November 13, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451468 "November 13, 2015 at 08:47")
    
    - ![](https://secure.gravatar.com/avatar/691b5bb2f0df8d2c07ade695df949319?s=160&d=retro&r=g)[
        
        ](https://fedoramagazine.org/systemd-using-journal/#comment-451521 "Comment by post author")
        
        #### [Paul W. Frields](http://pfrields.id.fedoraproject.org/)
        
        @marc: Fixed — thank you for catching that!
        
        [November 13, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451521 "November 13, 2015 at 16:28")
        
9. ![](https://secure.gravatar.com/avatar/645f01f4ddfffca4b502aaaa2920e1e6?s=160&d=retro&r=g)
    
    #### Erik
    
    Many thanks for putting these terse / helpful systemd essays together.
    
    [November 16, 2015](https://fedoramagazine.org/systemd-using-journal/#comment-451932 "November 16, 2015 at 17:40")
    

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

![](https://pixel.wp.com/g.gif?v=ext&blog=58617187&post=10231&tz=0&srv=fedoramagazine.org&j=1%3A12.3&host=fedoramagazine.org&ref=&fcp=0&rand=0.017379616286279598)

…