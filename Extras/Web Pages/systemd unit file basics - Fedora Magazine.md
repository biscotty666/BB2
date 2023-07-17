up:: [[SystemD]]
tags:: #note/reference  #source/internet #source/FedoraMagazine 
X:: 

## systemd unit file basics - Fedora Magazine


Welcome back to the systemd series, where we explore more about how this central part of your Fedora system works. This installment talks about _unit files_.

As a long-time Fedora user, I hadn’t thought much about systemd actually worked until recently. systemd is compartmentalized so that components of your system can be more easily managed. systemd uses _unit files_ to configure and manage system resources such as processes and your file system. Using these files you can wrangle systemd to manage your Fedora system the way the you want.

## Unit files: what are they?

The unit files on your system determine how systemd will start and run. Each corresponds to a single activity or component — or _unit_ in systemd terms. Each unit file is a simple text file describing a unit, what it does, what needs to run before or afterward, and other details.

Unit files can be stored in a few different places on your system. systemd looks for system unit files in this order:

1. /etc/systemd/system
2. /run/systemd/system
3. /usr/lib/systemd/system

Unit files in the earlier directories override later ones. This is a useful scheme, because it lets you make changes in the /etc directory, where configuration is expected. You should avoid making changes in /usr. Your system installs package data there that’s not expected to change.

systemd can also run in a user context, and manage resources per user in addition to system-side. Unit files for user units are stored similarly in _/etc/systemd/user, /run/systemd/user,_ and _/usr/lib/systemd/user._ The order of precedence works similarly.

You can examine some details about these unit files using the _systemctl_ command. If you want to see a list of all the unit files installed on the system, run this command:

systemctl list-unit-files

Each unit file contains options in the form _OptionName=value_, separated into sections marked like _[SectionName]._ These options describe how the unit works, and how systemd deals with it.

There are numerous types of units systemd understands. The two most common for system owners to deal with are _service_ units and _target_ units. To list unit files on your system of each of these types, use the _systemctl_ command:

systemctl list-unit-files --type service
systemctl list-unit-files --type target

## Service units

These are the units describe a process systemd can start and monitor. Service units are the most common units you’ll use daily. They are controlled using _systemctl_ as root:

sudo systemctl [command] _NAME_.service

Typical commands include:

- _start:_ starts a systemd unit
- _stop_: attempts to “nicely” end a service
- _status_: provides detailed information on a service
- _restart_: restarts (stops and then starts) the specified service
- _enable_: hooks (links) a unit to various places, for instance to run at boot
- _disable_: unhooks (unlinks) a unit, so it is not activated

The following example is the _sshd.service_ unit file. It allows systemd to control the _sshd_ daemon that allows remote access to your system via SSH (Secure Shell).

[Unit]
Description=OpenSSH server daemon
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target sshd-keygen.service
Wants=sshd-keygen.service

[Service]
EnvironmentFile=/etc/sysconfig/sshd
ExecStart=/usr/sbin/sshd -D $OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target

This unit features both some common unit file options, and service unit-specific options. Common options seen here include a description and where to find documentation. There is obviously more in this unit file than this article can explain. But as you can probably guess from the _ExecStart_ option, this unit runs the _sshd_ daemon when started.

## Service owned processes

One interesting feature of systemd is that it monitors processes it starts with service units. To find out what processes are being monitored, use the _systemctl status_ command. For instance, here is output from that command checking the status of the _sshd.service_ unit:

● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2015-10-20 14:51:24 EDT; 1 weeks 0 days ago
   Docs: man:sshd(8)
         man:sshd_config(5)
   Main PID: 1090 (sshd)
   CGroup: /system.slice/sshd.service
           └─1090 /usr/sbin/sshd -D

We can see that the process started by this service has process ID (PID) 1090. This process will continue to be monitored by systemd.

Notice also that the service has placed this process in a specially named _control group_. The control group (or “cgroup”) allows systemd to manage associated processes together. In future articles in the series, we’ll explore how this feature allows you tune performance and resource limits.

Thanks to knowing the processes owned by this service, systemd can also help you manage errant services. Normally when you stop a service, you’ll use the _systemctl_ command as mentioned earlier. For example, to stop the web server service:

systemctl stop httpd.service

But what if the service doesn’t respond or cooperate? In this case, systemctl has a built in _kill_ switch:

systemctl kill httpd.service

## Target units

Target units are used to link and group other units together to describe a desired system state. Some of these units may be services. Others may be additional target units with their own groups of units.

Here’s an example, the _multi-user.target_ unit file.

[Unit]
Description=Multi-User System
Documentation=man:systemd.special(7)
Requires=basic.target
Conflicts=rescue.service rescue.target
After=basic.target rescue.service rescue.target
AllowIsolate=yes

Notice that this target unit file doesn’t contain a command to execute. Instead, it functions as purely as a way to connect other units (in this case, mostly targets) together. In this case:

- The _multi-user.target_ requires that _basic.target_ run successfully when _multi-user.target_ is run.
- If the _rescue.service_ or _rescue.target_ units run, they will cause this unit to stop, and vice versa.
- The _multi-user.target_ unit starts after _basic.target_ starts, and after _rescue.service_ and _rescue.target_ are run, if those are started.

Additionally this target unit includes the _AllowIsolate_ option. This option allows your system to treat the _multi-user.target_ unit as a boot target, using the _systemctl isolate_ command.

Target units group other units together not just with their unit file contents. A target can also have a _.wants_ directory that links to units which will be started along with the target. For instance, the _/usr/lib/systemd/system/multi-user.target_ file has an associated folder _/usr/lib/systemd/system/multi-user.target.wants_. This directory contains links to units (not just services but other targets as well) that will run when this target is run. Each of those may have its own dependencies, or unit file options like Requires above, as well.

## Helpful references

For a deep dive on common Unit section options you can read the man page for units:

man systemd.unit

There are also specific documentation files for service and target files. You can read them with these commands:

man systemd.service
man systemd.target

If you’re looking for more information on process and control group management in systemd, refer to [this helpful blog entry](http://0pointer.de/blog/projects/systemd-for-admins-2.html).

### Like this:

Like Loading...

[For System Administrators](https://fedoramagazine.org/category/for-system-administrators/) [Using Software](https://fedoramagazine.org/category/software/)

[](https://fedoramagazine.org/author/iambryan/)![](https://secure.gravatar.com/avatar/3ea5470b384c6a9c6708f160b1824417?s=96&d=retro&r=g) 

#### [Bryan Sutherland](https://fedoramagazine.org/author/iambryan/)

### 7 Comments

1. ![](https://secure.gravatar.com/avatar/f50876dc489df65c750c9fa32f9dd33e?s=160&d=retro&r=g)
    
    #### Zbigniew Jędrzejewski-Szmek
    
    It’s not necessary, or recommended, to ever use systemctl kill to stop services. systemctl stop has a timeout of its own, and after this timeout is exceeded, it will proceed to kill -SIGKILL all processes of the service. Using systemctl kill also does not properly stop dependent processes beforehand.
    
    > treat the multi-user.target unit as a boot target, using the systemctl isolate command  
    > I think it would better to say ‘… using the systemctl set-default command’.
    
    [October 28, 2015](https://fedoramagazine.org/systemd-getting-a-grip-on-units/#comment-447338 "October 28, 2015 at 10:32")
    
2. ![](https://secure.gravatar.com/avatar/0d4b300b9c5a57a5b59601e06edaddd4?s=160&d=retro&r=g)
    
    #### ifoolb
    
    How to write log to systemd journal, use syslog.h?
    
    [October 31, 2015](https://fedoramagazine.org/systemd-getting-a-grip-on-units/#comment-448584 "October 31, 2015 at 07:11")
    
    - ![](https://secure.gravatar.com/avatar/f50876dc489df65c750c9fa32f9dd33e?s=160&d=retro&r=g)
        
        #### Zbigniew Jędrzejewski-Szmek
        
        That’s one way. Using syslog() you can write simple messages.
        
        You can also use sd_journal_print() to send structured log messages with additional fields.
        
        [October 31, 2015](https://fedoramagazine.org/systemd-getting-a-grip-on-units/#comment-448631 "October 31, 2015 at 09:47")
        
3. ![](https://secure.gravatar.com/avatar/417d5595b0004144a98ee102fcf5654c?s=160&d=retro&r=g)
    
    #### Tal
    
    I often find that with the emergence of any new sufficiently complex technology that I need to figure out, the initial phase of wrapping my head around the ideas behind the technology is the hardest part. You’ve made this very easy with your systemd articles. I’m currently going through them one by one.
    
    Thanks Bryan.
    
    PS:
    
    Reread the first sentence of the “Service units” section  
    Search this article for the words “as purely as a way”
    
    [November 14, 2015](https://fedoramagazine.org/systemd-getting-a-grip-on-units/#comment-451675 "November 14, 2015 at 12:26")
    
4. ![](https://secure.gravatar.com/avatar/650ecefa5583531217d3e7f7de928b40?s=160&d=retro&r=g)
    
    #### Bram
    
    In the example you are using the EnvironmentFile option to read options from a file in /etc/sysconfig.
    
    The systemd blog contains two articles that advice against using /etc/sysconfig or /etc/default:  
    – [http://0pointer.de/blog/projects/on-etc-sysinit.html](http://0pointer.de/blog/projects/on-etc-sysinit.html)  
    – [http://0pointer.de/blog/projects/the-new-configuration-files.html](http://0pointer.de/blog/projects/the-new-configuration-files.html)
    
    What alternatives are there to configure system specifc options for a systemd unit file?
    
    Thanks in advance
    
    Bram
    
    [February 23, 2016](https://fedoramagazine.org/systemd-getting-a-grip-on-units/#comment-464129 "February 23, 2016 at 17:33")
    
    - ![](https://secure.gravatar.com/avatar/f50876dc489df65c750c9fa32f9dd33e?s=160&d=retro&r=g)
        
        #### Zbigniew Jędrzejewski-Szmek
        
        Drop-ins (see [https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description)).
        
        [February 25, 2016](https://fedoramagazine.org/systemd-getting-a-grip-on-units/#comment-464233 "February 25, 2016 at 13:16")
        
    - ![](https://secure.gravatar.com/avatar/f50876dc489df65c750c9fa32f9dd33e?s=160&d=retro&r=g)
        
        #### Zbigniew Jędrzejewski-Szmek
        
        Drop-ins (see [https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description)).
        
        [February 25, 2016](https://fedoramagazine.org/systemd-getting-a-grip-on-units/#comment-464234 "February 25, 2016 at 13:18")
        

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

![](https://pixel.wp.com/g.gif?v=ext&blog=58617187&post=10226&tz=0&srv=fedoramagazine.org&j=1%3A12.3&host=fedoramagazine.org&ref=&fcp=0&rand=0.03617702903013953)