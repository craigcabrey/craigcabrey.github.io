---
layout: post
title: Guide to using U2F with GDM
image: /assets/article_images/2015-12-13-guide-to-using-u2f-with-gdm/header.jpg
---

# Introduction

[Universal 2-Factor Authentication](https://www.yubico.com/applications/fido/)
is an open authentication standard developed primarily by Yubico and Google.
The primary intention of U2F is to enable the use of simple hardware devices to
aid with 2-factor authentication on popular applications such as GitHub and
Google Apps.

However, U2F is not limited to just these applications. It can also be used to
enable 2-factor authentication within a custom application or even for use on a
local operating system. In this guide, I will show you how to use a U2F YubiKey
(many thanks to GitHub and Yubico for
[partnering together](https://www.yubico.com/github-special-offer/) on this)
with a Linux system to enable U2F for login and unlocking purposes.

# Getting Started

I’m using Arch Linux in combination with GNOME 3.18. While this guide is likely
to work with other systems (namely Fedora), be aware that some changes may be
required.

Additionally, it is assumed you have a U2F YubiKey (only a select few support
U2F, be sure to check this before purchasing or just use the GitHub offer
linked above). Other devices implementing U2F may work, however, I have not
tested this procedure with anything other than my own YubiKey.

# Software

For Arch, there is only a single package that needs to be installed. However,
this package is in the AUR, not the main repositories. It would be helpful to
use an AUR helper, although this is not required. In this example, I will be
using `pacaur`:

`pacaur -S pam_u2f`

When it prompts you to view the PKGBUILD, I encourage you to say **yes**. I
admit that while this should be the practiced followed for every AUR package, I
typically don’t do this. But as we are installing a module that directly
interacts with the authentication sub-system on your machine, you should
inspect the PKGBUILD to ensure nothing suspicious is occurring.

The above package will pull in a few dependencies such as `libu2f-host` and
`libu2f-server`. These libraries implement the U2F protocol and are required
for the PAM module to communicate with the hardware device.

At this point, you should plug in your device to verify that the kernel can
talk to it. Verify this by executing something like:

`dmesg | grep -i yubico`

If you don't see something like the following:

`hid-generic 0003:1050:0120.0003: hiddev0,hidraw0: USB HID v1.10 Device
[Yubico Security Key by Yubico] on usb-0000:00:14.0-2/input0`

then try rebooting. It’s possible that the udev rules have not kicked in after
package installation and rebooting will ensure they are loaded.

# Configuration

Once the packages have been successfully installed, there are two steps that
must be taken.

## Generating the U2F Config

A useful utility is included as part of the `pam_u2f` package we installed
earlier: `pamu2fcfg`. This utility is capable of producing a configuration file
that links your username and your particular U2F device that is read by the PAM
module.

Use it as follows:

`pamu2fcfg > ~/.config/Yubico/u2f_keys`

`pam_u2f` searches for this file in the authenticating user’s home directory by
default (although this can be changed with module parameters).

Touch the button on the device when it is blinking to complete the
configuration process.

## Configuring PAM

PAM (short for Pluggable Authentication Modules) is a mechanism for configuring
system authentication in Linux and other Unix/Unix-like operating systems.

`/etc/pam.d` is the directory where the various configurations live on a Linux
system. In particular, we’re interested in the file `/etc/pam.d/gdm-password`.
This is the file GDM (GNOME Display Manager) uses to determine authentication
techniques.

Open the file for editing with sudo and add the following to the top of the
file:

`auth sufficient pam_u2f.so`

There is something important to note with the second parameter of that
configuration, `sufficient`. This keyword lets PAM know what is required for
authentication to succeed. In this case, `sufficient` refers to the fact that
authenticating with U2F is *sufficient* for authentication to succeed, but it
is not *required*.

If you opt for using the `sufficient` level, you are not employing true
2-factor authentication. Instead, you must indicate `required`:

`auth required pam_u2f.so`

In this mode, the device will be *required* for authentication in addition to
whatever else is listed in the configuration (typically a password,
fingerprint, or smartcard, for example).

Read `pam(8)` and `pam.conf(5)` for more information on the specifics of PAM
control values.

# Finishing Up

At this point, you should have everything you need for U2F authentication with
GDM. Reboot and cross your fingers that you didn’t muck up your PAM
configuration and lock yourself out of your machine (although if that happens,
there are a few tricks to recover from that).

Thanks for reading!

