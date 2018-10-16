title: "Debian从backport升级到主线内核"
date: 2018-10-26 17:00:00 +0800
update: 2018-10-26 17:00:00 +0800
author: me
# cover: "-/images/ink-Travis.png"
tags:
    - devops
    - tips
preview: Debian从backport升级到主线内核

---

When using the latest version of Debian 9 stable, even with all updates installed, by default, you can’t get a very recent kernel via the standard repositories in your package manager. While the idea of using Debian stable is to remain stable and rather conservative, there are several benefits with installing a newer kernel and in some cases it’s the only option to get the OS to support all your hardware. The risk and impact on stability is small and the process is rather simple.

Some of the benefits are:
Support for previously unsupported hardware: every kernel release has a list of added drivers. Especially when you have recent hardware, a newer kernel could be required to fully support your video card for example.
Performance improvements and bug fixes: newer kernels often contain a lot of bug fixes, have new functions and performance tweaks. Here again, the most is to gain on newer hardware.
New kernel options and security fixes
The most recent (stable) kernel that is available at the moment of writing is version 4.12, released 27/07/2017. You can find a complete overview of changes in every kernel version at https://www.kernel.org/ or http://kernelnewbies.org/LinuxVersions

The latest version of the kernel available, at the time of writing, via the standard repositories for Debian 9 is/was. As you can see this kernel is quite recent but Stretch has just been released:

```bash
lyndon@debian:~$ cat /etc/debian_version
9.1
lyndon@debian:~$ uname -r
4.9.0-3-amd64
```

There are basically two options to install a newer kernel in Debian stretch. The first is the easiest and this is what I will explain in this post. The second is not so easy. It is simply to compile a newer kernel yourself. While compiling a kernel nowadays isn’t rocket science anymore, the first way is still preferable and will save you a lot of time because others have been through the second method and present you the result of their work :)

Installing a newer kernel in Debian Stretch
The easiest way to install a newer kernel in Debian, is to install it from the backports. Backports are packages taken from the next Debian release (called “testing”), adjusted and recompiled for usage on the stable release. For this post, I’m starting with a minimal install (system tools only), the only packages I added after finishing the installation were sudo and aptitude.

In order to install a kernel from the backports, we need to add the backports-repository for our Debian version to the apt-sources and update the list of available packages:

```bash
lyndon@debian:~$ echo "deb http://ftp.debian.org/debian stretch-backports main" | sudo tee -a /etc/apt/sources.list > /dev/null
lyndon@debian:~$ sudo apt-get update
...
Reading package lists... Done
```

Now, you can browse the available kernels in the backports-rpository by adding -t wheezy-backports to your apt commands.:

```bash
lyndon@debian:~$ aptitude search linux-image
p   linux-image-4.11.0-0.bpo.1-amd64                                        - Linux 4.11 for 64-bit PCs
p   linux-image-4.11.0-0.bpo.1-amd64-dbg                                    - Debug symbols for linux-image-4.11.0-0.bpo.1-amd64
p   linux-image-4.11.0-0.bpo.1-rt-amd64                                     - Linux 4.11 for 64-bit PCs, PREEMPT_RT
p   linux-image-4.11.0-0.bpo.1-rt-amd64-dbg                                 - Debug symbols for linux-image-4.11.0-0.bpo.1-rt-amd64
i A linux-image-4.9.0-3-amd64                                               - Linux 4.9 for 64-bit PCs
p   linux-image-4.9.0-3-amd64-dbg                                           - Debug symbols for linux-image-4.9.0-3-amd64
p   linux-image-4.9.0-3-rt-amd64                                            - Linux 4.9 for 64-bit PCs, PREEMPT_RT
p   linux-image-4.9.0-3-rt-amd64-dbg                                        - Debug symbols for linux-image-4.9.0-3-rt-amd64
i   linux-image-amd64                                                       - Linux for 64-bit PCs (meta-package)
p   linux-image-amd64-dbg                                                   - Debugging symbols for Linux amd64 configuration (meta-package)
p   linux-image-rt-amd64                                                    - Linux for 64-bit PCs (meta-package), PREEMPT_RT
p   linux-image-rt-amd64-dbg                                                - Debugging symbols for Linux rt-amd64 configuration (meta-package)
```
Before installing a newer kernel, it’s important that we upgrade installed packages to their newer versions in the backports-repo in order to be sure that dependencies will remain unbroken.

`lyndon@debian:~$ sudo apt-get -t stretch-backports upgrade`

After the upgrade, choose a kernel from the list and install it with apt. Most of the installations would want the kernel without suffix. The rt-suffix stands for realtime and is mostly interesting for embedded projects or machines that will drive industrial hardware. dbg stands for debugging.

```bash
lyndon@debian:~$ sudo apt-get -t stretch-backports install linux-image-4.11.0-0.bpo.1-amd64
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  firmware-linux-free irqbalance libglib2.0-0 libglib2.0-data libnuma1 shared-mime-info xdg-user-dirs
Suggested packages:
  linux-doc-4.11 debian-kernel-handbook
The following NEW packages will be installed:
  firmware-linux-free irqbalance libglib2.0-0 libglib2.0-data libnuma1 linux-image-4.11.0-0.bpo.1-amd64 shared-mime-info xdg-user-dirs
0 upgraded, 8 newly installed, 0 to remove and 2 not upgraded.
Need to get 45.4 MB of archives.
After this operation, 211 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
...
rocessing triggers for initramfs-tools (0.130) ...
update-initramfs: Generating /boot/initrd.img-4.11.0-0.bpo.1-amd64
Processing triggers for systemd (232-25+deb9u1) ...
After the installation, reboot your system and select the newly installed kernel from the selection displayed in GRUB. Debian selects the new kernel by default.’
```

To verify that the new kernel is used after booting:

```bash
lyndon@debian:~$ uname -r
4.11.0-0.bpo.1-amd64
```

Uninstalling unused kernels in Debian
When everything works as expected, you can safely uninstall the older kernel in order to clean up your system and to free up some space in /boot

To check which kernels are currently installed:

```bash
lyndon@debian:~$ dpkg --get-selections|grep linux-image
linux-image-4.11.0-0.bpo.1-amd64		install
linux-image-4.9.0-3-amd64			install
linux-image-amd64				install
```

Uninstall the old one:

```bash
lyndon@debian:~$ sudo apt-get remove linux-image-4.9.0-3-amd64
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following packages will be REMOVED:
  linux-image-4.9.0-3-amd64 linux-image-amd64
0 upgraded, 0 newly installed, 2 to remove and 0 not upgraded.
After this operation, 190 MB disk space will be freed.
Do you want to continue? [Y/n] y
(Reading database ... 31737 files and directories currently installed.)
Removing linux-image-amd64 (4.9+80+deb9u1) ...
Removing linux-image-4.9.0-3-amd64 (4.9.30-2+deb9u2) ...
I: /vmlinuz.old is now a symlink to boot/vmlinuz-4.11.0-0.bpo.1-amd64
I: /initrd.img.old is now a symlink to boot/initrd.img-4.11.0-0.bpo.1-amd64
/etc/kernel/postrm.d/initramfs-tools:
update-initramfs: Deleting /boot/initrd.img-4.9.0-3-amd64
/etc/kernel/postrm.d/zz-update-grub:
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-4.11.0-0.bpo.1-amd64
Found initrd image: /boot/initrd.img-4.11.0-0.bpo.1-amd64
done
```

That should be all it takes in order to install a recent kernel and cleanup older kernels on a Debian system. Not so difficult as you thought probably :)
