---
layout: post
permalink: /unable-to-scp-to-juniper-junos-devices-with-winscp
title: "Unable to SCP to Juniper JUNOS Devices with WinSCP"
category: Gotcha
tags: cli juniper juniper-networks junos putty scp sftp shell start-shell winscp networking ssh scp remote login
---
### Copying Files with SCP

When you are working within a network there is always a need to copy files. This includes patches, upgrades, scripts and logs which always have a need to be transfered. For \*nix platforms, the most common transfer protocol is [SCP](http://en.wikipedia.org/wiki/Secure_copy). SCP is short for secure copy which uses SSH as the transport mechanism to compress and encrypt data as it travels across a network.

Depending on your platform you are copying from, you can either use a CLI version of scp, or for Windows users, you can utilize [WinSCP](http://winscp.net/eng/index.php). WinSCP gives you an advanced configuration wizard to help you connect and transfer using a graphical user interface.

## Using WinSCP

One of the downfalls to using WinSCP is that Juniper Networks' JUNOS CLI is interpreted correctly. When you access a JUNOS device you have an option of two shells, one called shell and one called cli. The cli mode, accessed by typing `start cli` is the JUNOS configuration and management shell. The standard csh, accessed by `start shell` is the BSD backend version of a standard terminal shell where \*nix commands can be executed.

If you try to access a JUNOS platform with the user `root`, everything will work fine and dandy. This is because the default shell for `root` is `/bin/csh`. But, if you are like every other security-driven user out there, you would have disabled the `root` user as a safe-guard. This is where things get messy with WinSCP.

**Note:** Trying to log in using SFTP as the protocol will also work, but there are times when users need SCP by default.

Trying to log in as a non-root user via SCP will result in the following errors:

    Host is not communicating for more than 15 seconds. Still waiting...
    Note: If the problem repeats, try turning off 'Optimize connection buffer size'.

And

    Error skipping startup message. Your shell is probably incompatible with the application (BASH is recommended).

## Fixing the Issue

The fix is fairly simple. If you are familiar with PuTTy and advanced configuration, WinSCP is very similar.

The setting we need to change is hidden beneath the Advanced Options menu:

1. On the WinSCP Login Screen, check `Advanced Options` 
2. Under the `Environment` tree, choose `SCP/Shell` 
3. Look for the option `Shell`, the default option is `Default`
4. Change this to `start shell`

When you reconnect, WinSCP will pass `start shell` to the CLI and copy the files correctly.

