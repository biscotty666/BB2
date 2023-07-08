up::
tags:: #note/fleeting 
X:: 

## systemd- Converting sysvinit scripts - Fedora Magazine



---

### References
[![](https://fedoramagazine.org/wp-content/themes/fedoramagazine-1.15/images/fmag-ribbon.png)](https://fedoramagazine.org/)

## SysV init scripts

The init system used previously in Linux was called _SysVinit_. Before we start, it’s helpful to know that systemd has a lot of compatibility features for SysVinit. In some cases, you may not have to edit files to maintain your custom solutions. But in others, it’s helpful to know how to adapt to and use new systemd technology.

A script used to manage a service in SysVinit was known as a _SysV init script_. With some additions, it was known as a LSB, or Linux Standards Base, init script. Fear not, backwards compatibility for legacy scripts remains 99.9% intact with systemd. That means you don’t need to rush to convert all your scripts simply to adopt a systemd-based OS like Fedora. However, there are a number of advantages to doing so.

Init scripts are actually _imperative_ shell scripts executed by an interpreter. Here, “imperative” means they contain the commands the system runs to manage the service. However, systemd unit files are _declarative_ statements of intent. They give hints to systemd to manage a service, but systemd takes care of execution. As a result, unit files are typically much shorter than the SysV init scripts that they replace. That’s because they don’t have to worry about implementation, just the intent of the service.

The first step in migrating a SysV init script is to examine the script that you wish to convert. Let’s use the [sshd init script](http://pkgs.fedoraproject.org/cgit/openssh.git/tree/sshd.init?h=f16) from Fedora 16 as an example. The previous link will show you the script before it was converted to a systemd unit file. Here’s a fun fact: everything handled by that 184 line shell script is now handled by 27 lines of systemd configuration, spread across two unit files.

## Runlevels vs. targets

Reviewing the [init script](http://pkgs.fedoraproject.org/cgit/openssh.git/tree/sshd.init?h=f16) linked above, let’s look more closely at a few important parts. First is this line:

# chkconfig: 2345 55 25

The first set of numbers, _2345_, indicates in which _runlevels_ in the SysV world this script should run. A SysV runlevel is a definition of a system state where certain processes and services should run. There are a specific set of runlevels defined:

- 0: halt
- 1: single-user
- 2: multi-user
- 3: multi-user with networking
- 4: undefined (user defined)
- 5: multi-user with display manager (graphical login)
- 6: reboot

In systemd, there is no concept of runlevels. These are replaced by _targets_. In systemd, there can be an unlimited set of targets, each represented by a unit file with a _.target_ suffix. The analogous target to SysV runlevel 3 in systemd is called _multi-user.target._ Similarly, a unit corresponding to runlevel 5 is called _graphical.target_.

Targets can and do depend on each other. So only once the system has reached _multi-user.target_ does it start the services specified in _graphical.target._ There are also implicit dependencies on _basic.target_ which, as the name suggests, establishes basic system services and functions. Some of these happen through their own target dependencies, like _network.target_.

## Service ordering

The next two numbers in the SysV init script, _55_ and _25_, determine the order for starting and stopping the service. Under SysVinit, scripts run strictly in order. The order is determined by naming a set of links in folders. If a new script is placed in the wrong order to start or stop, the service may fail, the system may appear to hang, or other errors may result.

On the other hand, directives in systemd unit files like _Requires=_ and _After=_ ensure that units run in the correct order. They don’t require prior knowledge about other units or services on the system. For instance, if your unit runs a service that requires the network, you can add _Requires=network.target_ and _After=network.target_ to the unit file. It will only be run once the network is activated.

There are also dependencies included in the old SysV init script. However, they’re not enforced if you simply run the script outside the control of the init system. One of the major features of systemd is deterministic results from running an action.

## That’s a lot of code

Following header information, such as hard and soft dependencies (_Required-Start, Required-Stop, Should-Start_, and _Should-Stop_) is the description of the service. In this case that’s _Start up the OpenSSH server daemon_. Following are various functions that implement what the script does. The standards refer to certain _verbs_ the script must implement, such as _start_, _stop_, and _restart_.

As mentioned earlier, though, the systemd way of doing things is declarative. Rather than having to define and code a set of functions, you specify the process to execute. There’s no need, for example, for a large amount of code for a common task such as retrieving the process ID (PID) the service starts.

Let’s look at the start() function of this script to see what it does, and let’s examine it line by line. This is a _shell script,_ a program of instructions to be run by the shell started by the init system. Don’t worry if you don’t know the language; the instructions are explained below.

start()
{
    [ -x $SSHD ] || exit 5
    [ -f /etc/ssh/sshd_config ] || exit 6
    # Create keys if necessary
    /usr/sbin/sshd-keygen

    echo -n $"Starting $prog: "
    $SSHD $OPTIONS && success || failure
    RETVAL=$?
    [ $RETVAL -eq 0 ] && touch $lockfile
    [ $RETVAL -eq 0 ] && cp -f $XPID_FILE $PID_FILE
    echo
    return $RETVAL
}

In order, here’s what the init system does:

- Checks if the _sshd_ binary exists and is executable, and if not, exits with a status code of 5.
- Checks whether the configuration file _/etc/ssh/sshd_config_ exists, and if not, exits with status code 6
- Runs the _sshd-keygen_ binary to make keys, if they don’t already exist
- Sends a message to the screen/log that it’s about to start the service
- Runs the service, and outputs a success or failure message for startup of the _sshd_ process
- Captures a _return value_ for the _sshd_ process itself; 0 means _sshd_ has successfully become a service daemon
- Based on the return value, marks several marker files to indicate the service is running, in case the system administrator tries to run it again
- Sends out a “next line” message for the screen/log
- Passes the return value out in case it’s needed elsewhere

**Note this is only _one function_ in the SysV sshd init script**. Many other functions also need to be coded, including _stop_, _restart,_ and others. Those are over a hundred more lines of code! But what does the systemd unit file look like in comparison?

## systemd: cuts down on code

In contrast, here’s the _sshd.service_ unit file from a Fedora 23 installation. Notice how much smaller and easier to read this file is:

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

The first few lines again describe the systemd unit, and where you can read more about it. Here are what the remainder of the lines mean, with links to systemd documentation:

- _[After](http://www.freedesktop.org/software/systemd/man/systemd.unit.html#Before=)_ declares ordering, and describes that _network.target_ and _sshd-keygen.service_ should run first.
- _[Wants](http://www.freedesktop.org/software/systemd/man/systemd.unit.html#Wants=)_ describes that _sshd-keygen_ should be run in order for this service to start. Note the subtle difference from ordering. systemd considers order and dependencies _orthogonal_, meaning just because _sshd-keygen_ comes first doesn’t mean it is a dependency of _sshd_ — although in this case it is. _Wants_ means systemd should run _sshd-keygen.service,_ but if that doesn’t complete successfully (for instance, if SSH server keys already exist), _sshd_ will still run. If _sshd-keygen.service_ needed to complete successfully, you’d use _Requires_ instead.
- _[EnvironmentFile](http://www.freedesktop.org/software/systemd/man/systemd.exec.html#EnvironmentFile=),_ similar to SysVinit, is a configuration file with options for _sshd_. It contains a set of _key=value_ pairs that will be passed to _sshd._
- _[ExecStart](http://www.freedesktop.org/software/systemd/man/systemd.service.html#ExecStart=)_ is the command that runs to start _sshd_. This replaces the entire _start_ function in the old initscript. The _$OPTIONS_ variable here is what is specified in EnvironmentFile for the variable OPTIONS.
- _[ExecReload](http://www.freedesktop.org/software/systemd/man/systemd.service.html#ExecReload=)_ is the command that runs if the sysadmin reloads the _sshd_ daemon. This replaces the entire _reload_ function in the old initscript.
- _[KillMode](http://www.freedesktop.org/software/systemd/man/systemd.kill.html#KillMode=)_ sets how systemd will stop the service. In this case, systemd will stop the main _sshd_ process only. Any additional _sshd_ child processes will continue to manage their open SSH sessions. This is not an option used often, but it is important here. For instance, it stops you from killing your own SSH session when you run _systemctl__!_
- _[Restart](http://www.freedesktop.org/software/systemd/man/systemd.service.html#Restart=)_ determines how systemd manages the service if it stops unexpectedly. In this case, _on-failure_ means virtually any unexpected failure will cause systemd to restart _sshd_. If _sshd_ stops normally, though, such as with a _systemctl stop_ command, it will not be restarted.
- [RestartSec](http://www.freedesktop.org/software/systemd/man/systemd.service.html#RestartSec=) allows 42 seconds of sleep time before _sshd_ is restarted due to an unexpected stop.
- _[WantedBy](http://www.freedesktop.org/software/systemd/man/systemd.unit.html#WantedBy=)_ means when the command _systemctl enable sshd.service_ is run, a link is placed in the _multi-user.target.wants_ directory by default. This is one of the mechanisms systemd uses to determine units to run for specific targets. So _multi-user.target_ and any target that depends on it, will include running _sshd.service_.

There are of course many other directives available, and the systemd docs cover all of them. But if you were to write your own unit file for a simple service, you needn’t use even all the options shown here. You might only need a few directives like _After, ExecStart,_ and _WantedBy_. If you look in _/usr/lib/systemd/system/_ on your own Fedora system, you can find many other _.service_ unit files that you can follow as a template.

## Additional hints

Another notable feature of systemd, though, is that it doesn’t abandon compatibility for SysVinit. Some administrators have created their own custom startup scripts such as _/etc/rc.d/rc.local._ If you’re not prepared to convert it, don’t worry — systemd will honor it, using the _rc-local.service_ file already included.

Remember, if you are building custom system services for your Fedora, it’s best to place them into _/etc/systemd/system._ Typically _/etc/_ is where all system configuration or customization is kept. This allows you to avoid putting customizations in _/usr/lib/systemd/system_ which is managed by Fedora’s package system.

If you’d like to read another example and explanation of converting, check out [this blog entry](http://0pointer.de/blog/projects/systemd-for-admins-3.html) from the “systemd for administrators” series.

### Like this:

Like Loading...

[For System Administrators](https://fedoramagazine.org/category/for-system-administrators/) [Using Software](https://fedoramagazine.org/category/software/) [system administration](https://fedoramagazine.org/tag/system-administration/)[systemd](https://fedoramagazine.org/tag/systemd/)[sysvinit](https://fedoramagazine.org/tag/sysvinit/)

[](https://fedoramagazine.org/author/jstanley/)![](https://secure.gravatar.com/avatar/cdb505e95574dcdf1ca3d878c8f0cadf?s=96&d=retro&r=g) 

#### [Jon Stanley](https://fedoramagazine.org/author/jstanley/)

Jon is a longtime contributor to Fedora, and a former member of FESCo, the Fedora Board, as well as a current member of the Server Working Group. He lives in New York City, USA.

### 2 Comments

1. ![](https://secure.gravatar.com/avatar/106c1886d049ddedd104b358ca7d1fca?s=160&d=retro&r=g)
    
    #### Samuel Sieb
    
    Under the ExecStart section, there is an incomplete sentence:  
    “The $OPTIONS variable here is”
    
    [November 5, 2015](https://fedoramagazine.org/systemd-converting-sysvinit-scripts/#comment-450155 "November 5, 2015 at 14:16")
    
    - ![](https://secure.gravatar.com/avatar/cdb505e95574dcdf1ca3d878c8f0cadf?s=160&d=retro&r=g)[
        
        ](https://fedoramagazine.org/systemd-converting-sysvinit-scripts/#comment-450212 "Comment by post author")
        
        #### [jstanley](http://jstanley.id.fedoraproject.org/)
        
        Thanks! I can’t edit it at this point, but will bring it to the attention of folks that can.
        
        [November 5, 2015](https://fedoramagazine.org/systemd-converting-sysvinit-scripts/#comment-450212 "November 5, 2015 at 21:55")
        

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

![](https://pixel.wp.com/g.gif?v=ext&blog=58617187&post=10233&tz=0&srv=fedoramagazine.org&j=1%3A12.3&host=fedoramagazine.org&ref=&fcp=0&rand=0.367162831174463)