# Disclaimer

Customized ISOs for personal use are fine. If you want to share your customization with others, whether for free or for purchase, you'll have to rename it; remove all distro specific artwork, branding, and other identity elements; and you can't confuse your intended users about the customization being associated in any way with the base distro.

You're free to use the softwares without renaming those, as they are licensed under GPL. But otherwise, it will be your own creation and no longer you base distros'.

The name and identity elements of a distro are trademarked and copyrighted. Unless you have approval from appropriate authorization you can't use those (identity elements and name).

# What this is about:

This is a simple command line tool to customize live cd/dvd of ubuntu based distros, Linux Mint and some of their derivatives. It is developed with the help of the documentation found on:
https://help.ubuntu.com/community/LiveCDCustomization and intended primarily for personal use. This is released under GPL v2 lincense and redistrubtion is free and open complying to the licensing terms of GPL v2 license.

N.B: You need to modify the iso images on your own. It only renders an environment for modification and finally creates the modified iso image.

<mark>Please read through the <a href="#additional-info">Additional info</a> section before you start with a Ubuntu or Linux Mint ISO for the first time.</mark>

# Features:

1. You can save your project in a suitable directory and keep adding and changing things while checking the ISOs' built on those changes.
2. Your changes are always saved. You can resume it whenever you like.
3. It remembers your previous choice (the project directory, the desired ISO name and the original ISO path). Just hit <kbd>Enter</kbd> when you are prompt for input in such cases.
4. You only need to give it the original ISO once, every time after that, you can just go to the chroot terminal and keep customizing things.
5. It remembers user choices for various options and prompts both globally and locally (project wise).


# Requirements:

1. bash (This is generally installed by default in most Linux distros)
2. squashfs-tools
3. genisoimage
4. syslinux, syslinux-utils (If you want hybrid ISO image)
5. xterm (optional)

Install requirements with the following command:

```sh
sudo apt-get install squashfs-tools genisoimage syslinux syslinux-utils
```

Optionally you can keep `xterm` installed. It will work as a backup terminal in case the default terminal fails to run.

```sh
sudo apt-get install xterm
```

# Installation:

give the `install.sh` file execution permission and run it in terminal.

```sh
chmod +x ./install.sh
./install.sh    # or you can just drag & drop in terminal
```

# How to use:

Run `JLstart` in a terminal or run it from `menu->system->JLIVECD`.

<img alt="JLIVECD menu image" src="img/runjlivecd.png"></img>

Example:

```sh
~$ JLstart

Is this a new project: (y/n)?: n

[sudo] password for user:

...............................
```

Hints are given on the go, follow them to successfully create a customized live cd/dvd.

# Directories & Files:

1. In your project directory, you will find some default directories. Don't change their names. The directories are:

 1. `debcache`: `.deb` files are kept here. See the special feature section for more details.
 2. `edit`: This is the root filesystem (i.e `/`) for the live system (chroot system). Any change you make here will appear in the finalized ISO.
 3. `extracted`: This is where the original ISO is extracted. You can change several things here, like Diskname, release, date, splash screen, etc.
 4. `mnt`: A directory used only for mounting ISO image.
 
2. There's also an additional file named `.config`, which contains configuration of the corresponding project i.e DiskName and some other defaults for various options.



# Things to care:

1.Don't use quotation in file/folder path

`~/"some folder"` or `"~/some folder"` is invalid

`~/some folder` is valid

2.Don't use spaces in project path.

3.In a new project, don't close the terminal when it is extracting the original ISO. You can close it safely after it finishes extracting and the chroot is closed and another prompt for input is appeared.

4.Don't close the chroot and host terminal simultaneously. You can close the host terminal safely after an input prompt appears after closing the chroot terminal.

5.The default answer is `no` for all `yes/no` type questions unless specified otherwise.

6.The default answers for `yes/no` type questions are changed according to previous choices for some questions (retain home directory? etc..). For example, if you choose `y` for the question `retain home directory (y/n)?`; next time if you just hit <kbd>Enter</kbd>, it will take `y` instead of `n`. This is project specific i.e each project remembers its own options.

# Some Tips & Tricks:

1. If you are not being able to get connected to internet in chroot, you can try running the code: `JLopt -rn` in another terminal in your main system. This may happen if you start JLIVECD before connecting your pc to the internet.

2. If you want to change the timeout value then run this code in a terminal in your main system: `JLopt -t timeout_value`. "timeout_value" should be replaced with your desired time in seconds. Ex: for 12 seconds timeout: `JLopt -t 12`

3. JLIVECD seems to have problem running the `mate-terminal` properly. For mate DE, install `xterm` instead ( `sudo apt-get install xterm`).

4. You can change the default terminal JLIVECD uses for chroot. To change the primary default terminal run this code in a terminal in your main system: `JLopt -t1 actual-terminal-command`. To change the secondary default terminal: `JLopt -t2 actual-terminal-command`. For Ex. `JLopt -t1 gnome-terminal`

# Special Feature:

I call it debcache management! 

1. Just put your `.deb` files in *edit/var/cache/apt/archives* folder and they won't be downloaded again in the software installaion process.
2. They will be moved automatically to a folder named debcache (located in the same directory as "edit") prior to image creation so that they won't be included in the iso image.
3. You never need to delete .deb files from *edit/var/cache/apt/archives* manually and you shouldn't.
4. If you don't delete the .deb files then you will never need to download them again as long as they remain the updated files according to your package list (which you get from `apt-get update`). debcache management will take proper measures to move the files to required places to minimize downloads of packages from internet.
5. Altenatively, you can put the `.deb` files in **debcache** folder too, but in that case you need to run the application after you have finished copying files to this folder...

# ChangeLog:

###version 2.1.3

1. Consistency among hybrid and UEFI image.

###version 2.1.2:

1. UEFI image support
2. Hybrid image support

###version 2.1.1:

1. fix `$HOME` and `$LC_ALL` in chroot
2. chroot preparation is done in the same terminal window as chroot.
3. Aaccess control management (xhost)

###version 2.1.0:

1. You can change default terminals without applying patch to the source code.
2. It now remembers several project-wise options (delete home directory?, fast compression?, etc..).
3. Options are handled with config files both globally and project-wise.
4. Added show version info (`JLopt -v`)
5. Added show help menu (`JLopt -h`)
6. New script `JLopt` contains several useful functionality.
7. Several potential bug fixes.
8. Docs updated.


###version 2.0.6:

1. `xterm` is added as a secondary terminal besides the default `x-terminal-emulator`.
2. Docs updated.

###version 2.0.5:

1. New install or update of this tool will not delete the history i.e hitting <kbd>Enter</kbd> to take the previous choice won't be affected. This was first implemented in version 2.0.4, i.e version >=2.0.4 can be safely updated to any later version.
2. Added another compression method (fast compression).
3. Minor potential bug fixes.
4. Docs updated.

###version 2.0:

1.You can use short cut in names for path to base iso i.e xubuntu for xubuntu-14.04.1-x64.iso, if there is no other file named "xubuntu" in the same folder. You can even use only x if there is no other file starting with x in the same folder.

Example:

```sh
enter base iso path: ~/Downloads/x
```

As there is only one file that matches x is xubuntu-14.04.1-x64.iso, it will take that file as input automatically.

2.You can use full path with or without `.iso`.

# Tested OS:

* Xubuntu 16.04 LTS
* Linux Mint 17 cinnamon
* Linux Mint 17 XFCE
* Xubuntu 14.04.1 LTS
* Ubuntu 14.04.1 LTS
* Ubuntu 14.04.3 LTS

<div id="additional-info"></div>

# Additonal info:

1.In Linux Mint 17 XFCE there's a bug. To fix this edit `/usr/sbin/invoke-rc.d` file (in chroot) as:
replace `exit 100` with `exit 0` at line `285` and `421`, then apply upgrade. after upgrading revert this modification (must).

2.In Linux Mint 17 xfce, if you install nautilus then it will set gnome-session as default session and if gnome desktop is not installed then no desktop window will show up in live session. change the link `/usr/bin/x-session-manager` to point to `/usr/bin/xfce4-session`.

3.In xubuntu 14.04.1 there's another bug: Can't open /scripts/casper-functions" error) to fix this, run this code in chroot:

```sh
ln -s /usr/share/initramfs-tools/scripts /scripts
```

Follow the following link for bug report:

https://bugs.launchpad.net/ubuntu/+source/systemd/+bug/1325142

4.In Ubuntu 14.04 Gnome LTS you might encounter two more bugs: 

One should be solved by editing:

```sh
/var/lib/dpkg/info/whoopsie.prerm
/var/lib/dpkg/info/libpam-systemd\:amd64.prerm
/var/lib/dpkg/info/libpam-systemd\:amd64.postinst
```

(change `exit $?` to `exit 0` in the invoke-rc.d lines)

Other one should be solved by editing:

```sh
/etc/kernel/postrm.d/zz-update-grub
/etc/kernel/postinst.d/zz-update-grub
```

find the following and comment out the if and fi line: 

```sh
if [ -e /boot/grub/grub.cfg ]; then
   #exec update-grub
fi
```

Revert these changes before exiting the chroot.

Follow the following link for bug report for more details:

https://bugs.launchpad.net/ubuntu/+source/systemd/+bug/1325142

5.You may encounter another bug: `Ubiquity installer, hang/freeze on harddisk detection`. This bug can be solved by editing the file `edit/usr/share/applications/ubiquity-gtkui.desktop` and changing the section `exex` from

```sh
sh -c 'ubiquity gtk_ui'
```

to 

```sh
sh -c 'sudo ubiquity gtk_ui'
```
