Source:: [NFS - NixOS Wiki](https://nixos.wiki/wiki/NFS)
up:: [[NixOS]]
tags:: #note/reference #source/documentation #source/internet 

# NFS

From NixOS Wiki

Jump to: [navigation](#mw-navigation), [search](#p-search)

## Server

Let's say that we've got one server-machine with 4 directories that we want to share: `/mnt/kotomi`, `/mnt/mafuyu`, `/mnt/sen` and `/mnt/tomoyo`.

First, we have to create a dedicated directory from which our NFS server will access the data:


```
$ mkdir /export
```

You may need to change ownership of the `/export` directory to `nobody:nogroup`

Then we have to either move our already-existing directories inside `/export` (using `mv` from the command line) or bind-mount them there:

```
{
  fileSystems."/export/mafuyu" = {
    device = "/mnt/mafuyu";
    options = [ "bind" ];
  };

  fileSystems."/export/sen" = {
    device = "/mnt/sen";
    options = [ "bind" ];
  };

  fileSystems."/export/tomoyo" = {
    device = "/mnt/tomoyo";
    options = [ "bind" ];
  };

  fileSystems."/export/kotomi" = {
    device = "/mnt/kotomi";
    options = [ "bind" ];
  };
}
```

Having the filesystem ready, we can proceed to configure the NFS server itself:


```
{
  services.nfs.server.enable = true;
  services.nfs.server.exports = ''
    /export         192.168.1.10(rw,fsid=0,no_subtree_check) 192.168.1.15(rw,fsid=0,no_subtree_check)
    /export/kotomi  192.168.1.10(rw,nohide,insecure,no_subtree_check) 192.168.1.15(rw,nohide,insecure,no_subtree_check)
    /export/mafuyu  192.168.1.10(rw,nohide,insecure,no_subtree_check) 192.168.1.15(rw,nohide,insecure,no_subtree_check)
    /export/sen     192.168.1.10(rw,nohide,insecure,no_subtree_check) 192.168.1.15(rw,nohide,insecure,no_subtree_check)
    /export/tomoyo  192.168.1.10(rw,nohide,insecure,no_subtree_check) 192.168.1.15(rw,nohide,insecure,no_subtree_check)
  '';
}
```

This configuration exposes all our shares to 2 local IPs; you can find more examples at Gentoo's wiki [[1]](https://wiki.gentoo.org/wiki/NFSv4).

Other options are available on the [NixOS option page](https://search.nixos.org/options?query=nfs) or via the `nixos-option` command.

### Firewall

If your server-machine has a firewall turned on (as NixOS does by default, for instance), don't forget to open appropriate ports; e.g. for NFSv4:

networking.firewall.allowedTCPPorts = [ 2049 ];

Many clients only support NFSv3, which requires the server to have fixed ports:

  services.nfs.server = {
    enable = true;
    # fixed rpc.statd port; for firewall
    lockdPort = 4001;
    mountdPort = 4002;
    statdPort = 4000;
    extraNfsdConfig = '''';
  };
  networking.firewall = {
    enable = true;
      # for NFSv3; view with `rpcinfo -p`
    allowedTCPPorts = [ 111  2049 4000 4001 4002 20048 ];
    allowedUDPPorts = [ 111 2049 4000 4001  4002 20048 ];
  };

## Client

Continuing the server example, mounting the now-exposed _tomoyo_ share on another box (on a client) is as simple as:

{
  fileSystems."/mnt/tomoyo" = {
    device = "server:/tomoyo";
    fsType = "nfs";
  };
}

Replace "server" in the above device attribute with the IP address or DNS entry of the NFS server. Note that clients see exposed shares as if they were exposed at the root level - i.e. `/export/foo` becomes `/foo` (in the `device` option). Other, regular **fileSystems** options apply.

### Specifying NFS version

You can specify NFS version by adding the `"nfsvers="` option:

{
  fileSystems."/mnt/tomoyo" = {
    # ...
    options = [ "nfsvers=4.2" ];
  };
}

### Lazy-mounting

By default, all shares will be mounted right when your machine starts - apart from being simply unwanted sometimes, this may also cause issues when your computer doesn't have a stable network connection or uses WiFi; you can fix this by telling systemd to mount your shares the first time they are _accessed_ (instead of keeping them mounted at all times):

{
  fileSystems."/mnt/tomoyo" = {
    # ...
    options = [ "x-systemd.automount" "noauto" ];
  };
}

### Auto-disconnecting

You can tell systemd to disconnect your NFS-client from the NFS-server when the directory has not been accessed for some time:

{
  fileSystems."/mnt/tomoyo" = {
    # ...
    options = [ "x-systemd.idle-timeout=600" ]; # disconnects after 10 minutes (i.e. 600 seconds)
  };
}

### Using systemd.mounts and systemd.automounts

Here is an example with auto-disconnecting and lazy-mounting implemented, and the `noatime` mount option added.

Note that `wantedBy = [ "multi-user.target" ];` is required for the automount unit to start at boot.

Also note that `x-systemd` mount options are unneeded, as they are a representation of systemd options in `fstab(5)` format. They get parsed and converted to unit files by `systemd-fstab-generator(8)` as mentioned in `systemd.mount(5)`.

{
  services.rpcbind.enable = true; # needed for NFS
  systemd.mounts = [{
    type = "nfs";
    mountConfig = {
      Options = "noatime";
    };
    what = "server:/tomoyo";
    where = "/mnt/tomoyo";
  }];

  systemd.automounts = [{
    wantedBy = [ "multi-user.target" ];
    automountConfig = {
      TimeoutIdleSec = "600";
    };
    where = "/mnt/tomoyo";
  }];
}

Multiple mounts with the exact same options can benefit from abstraction.

{
  services.rpcbind.enable = true; # needed for NFS
  systemd.mounts = let commonMountOptions = {
    type = "nfs";
    mountConfig = {
      Options = "noatime";
    };
  };

  in

  [
    (commonMountOptions // {
      what = "server:/tomoyo";
      where = "/mnt/tomoyo";
    })

    (commonMountOptions // {
      what = "server:/oyomot";
      where = "/mnt/oyomot";
    })
  ];

  systemd.automounts = let commonAutoMountOptions = {
    wantedBy = [ "multi-user.target" ];
    automountConfig = {
      TimeoutIdleSec = "600";
    };
  };

  in

  [
    (commonAutoMountOptions // { where = "/mnt/tomoyo"; })
    (commonAutoMountOptions // { where = "/mnt/oyomot"; })
  ];
}

## Nix store on NFS

In a single-user setup (**not on Nixos**) the Nix store can be also exported over NFS (common in HPC clusters) to share package over the networks. The only requirement is to also pass `local_lock=flock` or `local_lock=all` as mount option to allow the nix packages to take locks on modifications. Example entry in `fstab`:

<host_or_ip>/nix /nix nfs nofail,x-systemd.device-timeout=4,local_lock=all 0 0

Retrieved from "[https://nixos.wiki/index.php?title=NFS&oldid=9081](https://nixos.wiki/index.php?title=NFS&oldid=9081)"

[Discussion](/index.php?title=Talk:NFS&action=edit&redlink=1 "Discussion about the content page [alt-shift-t]")

## Contents[to top](javascript:scrollTo(0,0);)

 [hide] 

- [1 Server](#Server)
    - [1.1 Firewall](#Firewall)
- [2 Client](#Client)
    - [2.1 Specifying NFS version](#Specifying_NFS_version)
    - [2.2 Lazy-mounting](#Lazy-mounting)
    - [2.3 Auto-disconnecting](#Auto-disconnecting)
    - [2.4 Using systemd.mounts and systemd.automounts](#Using_systemd.mounts_and_systemd.automounts)
- [3 Nix store on NFS](#Nix_store_on_NFS)

- [Privacy Policy](/wiki/NixOS_Wiki:Privacy_policy "NixOS Wiki:Privacy policy")
- [About NixOS Wiki](/wiki/NixOS_Wiki:About "NixOS Wiki:About")

- [Login / Create Account](/index.php?title=Special:UserLogin&returnto=NFS "You are encouraged to log in; however, it is not mandatory [alt-shift-o]")

[View source](/index.php?title=NFS&action=edit "This page is protected.
You can view its source [alt-shift-e]")

[Discussion](/index.php?title=Talk:NFS&action=edit&redlink=1 "Discussion about the content page [alt-shift-t]")

[Log in](#)

- [With a wiki account](/index.php?title=Special:UserLogin&returnto=NFS "You are encouraged to log in; however, it is not mandatory [alt-shift-o]")
- [Login with GitHub](/index.php?title=Special:OAuth2Client/redirect&returnto=NFS)