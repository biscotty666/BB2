up::
tags:: #note/fleeting 
X:: 

## systemd- Unit dependencies and order - Fedora Magazine



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

# systemd: Unit dependencies and order

Posted by [Paul W. Frields](https://fedoramagazine.org/author/pfrields/ "Posts by Paul W. Frields") on [November 25, 2015](https://fedoramagazine.org/systemd-unit-dependencies-and-order/)

![](https://fedoramagazine.org/wp-content/uploads/2015/10/systemd-deps-945x400.jpg)

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

Welcome back to our continuing series on systemd features. As you’ve guessed [from](https://fedoramagazine.org/what-is-an-init-system/) [our](https://fedoramagazine.org/systemd-getting-a-grip-on-units/) [previous](https://fedoramagazine.org/systemd-converting-sysvinit-scripts/) [articles](https://fedoramagazine.org/systemd-using-journal/), [systemd](https://fedoramagazine.org/systemd-masking-units/) brings more power and flexibility to service startup and management. One of the most important changes in systemd from legacy [SysVinit](https://en.wikipedia.org/wiki/Init) is how it starts up units.

You may have heard from casual users that systemd starts everything together. Some people believe this is true, and that’s why the system starts faster. But the reality is not quite that simple. Let’s look a little more deeply at how systemd understands unit relationships.

## Unit dependencies

Unit files include the feature of _dependencies_. Any unit may _want_ or _require_ one or more other units before it can run. These dependencies are set in unit files with the directives _[Wants](http://www.freedesktop.org/software/systemd/man/systemd.unit.html#Wants=)_ and _[Requires](http://www.freedesktop.org/software/systemd/man/systemd.unit.html#Requires=)_. The difference between these is simple.

- If unit1 has _Wants=unit2_ as a dependency, when unit1 is run, unit2 will be run as well. But whether unit2 starts successfully does not affect unit1 running successfully.
- When unit1 has _Requires=unit2_, however, again both units will run, but if unit2 does not succeed, unit1 is also deactivated. This happens regardless of whether the processes of unit1 would otherwise have worked fine.

Did you notice something subtle about this description? It doesn’t talk about order. When systemd starts your system, it loads all unit files and reads through them to determine dependencies like this. When unit1 runs in these examples, unit2 is run **at the same time**. It’s important to know that dependencies and ordering are two different things to systemd.

Here’s an example of dependencies in part of the _sshd.service_ unit file:

Wants=sshd-keygen.service

So when _sshd.service_ runs, the _sshd-keygen.service_ will run as well. However, _sshd-keygen.service_ does not need to succeed for _sshd.service_ to run successfully.

Why would a dependency like this need to exist? In this case, the _sshd-keygen.service_ creates new SSH keys for a server if they do not exist. We always want to check whether these keys exist before the SSH server starts. If they already exist, the service unit exits with an error indicating so. But in that case of course, we still want the SSH server to run as usual.

## Unit order

This doesn’t mean systemd can’t put things in proper order. Without any other instructions, systemd would run a group of units at the same time. This is probably why some people believe systemd starts everything at the same time (or “in parallel”). It is sometimes necessary, of course, for processes to run in a certain order.

Fortunately, systemd also has unit directives for this issue as well, _[Before](http://www.freedesktop.org/software/systemd/man/systemd.unit.html#Before=)_ and _[After](http://www.freedesktop.org/software/systemd/man/systemd.unit.html#Before=)._ These directives work pretty much as you’d expect:

- If unit1 has the directive _Before=unit2,_ then if both units are run, unit1 will be executed fully before unit2 starts.
- If unit1 has the directive _After=unit2,_ then if both units are run, unit2 will be executed fully before unit1 starts.

Once again, note how this ordering does not affect dependencies. Neither case causes unit2 to run. Let’s look at the _sshd.service_ unit again:

Wants=sshd-keygen.service
After=network.target sshd-keygen.service

The ordering directive _After_ ensures that the SSH server will not run until after the host key generation unit, and after the network is up. (The _network.target_ unit ensures that various units bring up the network.) Dependencies like _Wants_ and _Requires_ are often used together with _After_ to preserve proper dependencies _and_ order together.

## Faster boot time

You may recall from [earlier in this series](https://fedoramagazine.org/systemd-converting-sysvinit-scripts/) that SysVinit started every service in sequence, based on numbering. However, systemd automatically sorts through unit files to read dependency and ordering information. It uses this information to allow many services to start almost simultaneously, while preserving order where needed.

This handling is one reason startup can be faster under systemd. Faster boot time isn’t primarily why systemd was invented, but it’s often a side benefit of how it handles units.

### Like this:

Like Loading...

[For System Administrators](https://fedoramagazine.org/category/for-system-administrators/) [Using Software](https://fedoramagazine.org/category/software/)

[](https://fedoramagazine.org/author/pfrields/)![](https://secure.gravatar.com/avatar/691b5bb2f0df8d2c07ade695df949319?s=96&d=retro&r=g) 

#### [Paul W. Frields](https://fedoramagazine.org/author/pfrields/)

Paul W. Frields has been a Linux user and enthusiast since 1997, and joined the Fedora Project in 2003, shortly after launch. He was a founding member of the Fedora Project Board, and has worked on docsc, websites, advocacy, toolchain, and package maintenance. He joined Red Hat as Fedora Project Leader from February 2008 to July 2010, and remains with Red Hat as an engineering manager. He currently lives with his wife and two children in Virginia where he also runs a recording studio (5thdom.com).

### 5 Comments

1. ![](https://secure.gravatar.com/avatar/01293eb9c969f75cee0a806f5f301cf7?s=160&d=retro&r=g)
    
    #### [Stan Sykes](http://www.metcomm.com)
    
    Just a quick note to say a big “Thank You” for this series of articles. Very useful and informative.
    
    Kind regards,  
    Stan
    
    [November 25, 2015](https://fedoramagazine.org/systemd-unit-dependencies-and-order/#comment-453533 "November 25, 2015 at 05:27")
    
2. ![](https://secure.gravatar.com/avatar/dfad6adc8edc3b1bbe1dc16f899ee11a?s=160&d=retro&r=g)
    
    #### Steven Snow
    
    Hi Paul,
    
    Thank you for these articles. It is nice to understand the under the hood bits of Fedora to better customize it. Keep it up!
    
    [November 25, 2015](https://fedoramagazine.org/systemd-unit-dependencies-and-order/#comment-453597 "November 25, 2015 at 11:12")
    
3. ![](https://secure.gravatar.com/avatar/dcd4e62c4b1402ae6f152ffcba62e9b1?s=160&d=retro&r=g)
    
    #### David
    
    Unfortunately, in newer versions of systemd, network.target doesn’t work as an ordering milestone, and you have to use network-online.target instead if you want it to not start until the network connection is active. There are still some unit files that need to be fixed to adapt to this strange change.
    
    [November 25, 2015](https://fedoramagazine.org/systemd-unit-dependencies-and-order/#comment-453634 "November 25, 2015 at 15:30")
    
    - ![](https://secure.gravatar.com/avatar/106c1886d049ddedd104b358ca7d1fca?s=160&d=retro&r=g)
        
        #### Samuel Sieb
        
        If you think there is some unit that requires the network to be fully online before starting and doesn’t have the right target, then please file a bug to get it fixed.
        
        [December 3, 2015](https://fedoramagazine.org/systemd-unit-dependencies-and-order/#comment-454797 "December 3, 2015 at 19:01")
        
4. ![](https://secure.gravatar.com/avatar/788def1484e62941d45004963411c3e1?s=160&d=retro&r=g)
    
    #### Leslie Satenstein
    
    Paul, thank you for such a clear explanation about some of the “design jewels” created within the systemd architecture. When a design is just “Wow!, great!”, I call it a jewel.
    
    [November 25, 2015](https://fedoramagazine.org/systemd-unit-dependencies-and-order/#comment-453649 "November 25, 2015 at 17:56")
    

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

![](https://pixel.wp.com/g.gif?v=ext&blog=58617187&post=10228&tz=0&srv=fedoramagazine.org&j=1%3A12.3&host=fedoramagazine.org&ref=&fcp=0&rand=0.9732308118299242)