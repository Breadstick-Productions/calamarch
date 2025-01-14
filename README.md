# CALAMARCH KDE PLASMA

Join if you have problems: https://discord.gg/RD7XFBzBZZ 

Guys here's the drive link: https://drive.google.com/drive/folders/1ParhxOUlfZlA_raqt4vCkSyZjUPYN2Uf?usp=sharing 

If somehow `chaotic` doesn't work for you, you need to do this:

```
pacman-key --recv-key FBA220DFC880C036 --keyserver keyserver.ubuntu.com; pacman-key --lsign-key FBA220DFC880C036
```
Issues may be reported in this repo for tracking.
## Features
- Chaotic AUR and multilib enabled
- Wine installed
- Bundle of software
- Printing
## TBA in next month's build:
...

# Tips and Tricks (IMPORTANT, PLEASE READ !)

**When updating system**

- Always carry a Live USB with an iso in case something happens
- Make sure your laptop is connected to power
- You need a stable internet connection
- **Never interrupt a system update, make sure it runs until finish**
- If something happens, you need to boot into live usb, mount your system partition, and [arch-chroot](https://wiki.archlinux.org/title/chroot#Using_arch-chroot) into it. Run `pacman -Syu` over there. If you are still confused, you can either visit the [forums](https://bbs.archlinux.org) or the community [discord](https://discord.gg/HTEzWDF) 

**Games disabling compositor/desktop effects**

Go to `System Settings > Display and Monitor > Compositor` and uncheck "Allow applications to block compositing"

**I'm using dual GPUs (run `neofetch` in terminal and check if you have 2 GPUs)**

install `optimus-manager-qt` through Add/Remove Software.

**I'm not using NVIDIA, but using AMDGPU**

Install `amdvlk` and `lib32-amdvlk`

If you're using intel graphics, install `vulkan-intel` and `lib32-vulkan-intel`

In general, multiple Vulkan drivers can coexist, however with NVIDIA sometimes the NVIDIA Vulkan drivers may not be detected if other drivers are installed. Make sure you are using either "Hybrid" or "NVIDIA/AMD" graphics mode through Optimus Manager on a dual GPU laptop.

It is always good to install the 32-bit packages too (WINE requires them)

https://wiki.archlinux.org/title/Vulkan#Installation 

**Wifi not working**

Use USB tethering with your phone and install the correct drivers. Broadcom drivers are already installed as part of the package.

**My step-by-step problem solving for WINE problems (Lutris edition)**

- First, I enable/disable DXVK
- Then, I enable/disable NVIDIA prime offload settings
- Also, if you're using a laptop with two GPUs, switch the NVIDIA and use that as default
- If all else fails, I search the web for solution
- For last resort, I submit an issue on GitHub or forums. 
# Picture(s)
![image](https://user-images.githubusercontent.com/95167946/194882012-24f8f209-5673-4369-8d95-606a7cadff9f.png)

# Developer's manual for building:

In order to build `calamarch`, you need to update your system first:
```
sudo pacman -Syu
```
Then, clone the `alci-pkgbuild` repo and cd into the calamares PKGBUILD directory. Run a `makepkg -s` then put the `pkg.tar.zst` in `/path/to/calamarch/alci_local_repo/x86_64`. 

Run the `update-database.sh` file in `calamarch/alci_local_repo`. Then, edit the file `calamarch/archiso/pacman.conf` file to include the local repo (change the /home/path/to/calamarch) to where you store this folder (`calamarch`). Leave the ones with `$` alone.  

The real building process begins. Head into the `installation-scripts` directory. Run script 30 if you're building for the first time and run script 40 if you already built the iso once.

Test the iso in Virtualbox.

## Which files can I modify

The `archiso/packages.x86_64` file contains all the packages that will be installed on the iso. Note that the iso does not require an internet connection. The `archiso/pacman.conf` is the pacman configuration when you build the iso, it is not the same on the installed system. The actual `pacman.conf` file that will be installed is located inside `archiso/airootfs/etc/`, which also includes other configurations that will be installed on your system.

# Arch Linux Calamares Installer or ALCI

Use the correct version of Archiso to build the iso.

**Read the archiso.md.**

Download the content of the github with (use the terminal)

`git clone https://github.com/arch-linux-calamares-installer/alci-iso`

# Pacman.conf in archiso folder

Only the archiso/pacman.conf will be used to download your packages.

You can activate more sources besides Arch Linux repos

    arcolinux
    chaotic
    your own local repo



# Pacman.conf in archiso/airootfs/etc/

This will be your future system. 
Include the repositories you want.
It will not be used to build the iso.


# Keys and Mirrors

## ArcoLinux keys and mirror

Add the ArcoLinux keys and Arcolinux mirrors to the packages.x86_64.
The pacman-init service  at etc/systemd/system/pacman-init.service will add any keys present.


## Chaotic keys and mirror

Add the Chaotic keys and Chaotic mirrors to the packages.x86_64.
The pacman-init service  at etc/systemd/system/pacman-init.service will add any keys present.


# Archiso/packages.x86_64

Only the archiso/packages.x86-64 files will be used.

Add more packages at the bottom of the file

If you plan to use ArcoLinux packages

* arcolinux-keyring

* arcolinux-mirror

If you plan to use Chaotic packages

* chaotic-keyring

* chaotic-mirrorlist

You can even add packages from your own personal local repo.


If you know you are going to need drivers for graphical cards or NICs put them on the iso.
I am thinking about xf86-video-intel, nvidia or other drivers.

# Build process

Install these two packages on your system if you want to include **Chaotic packages** on the iso

`sudo pacman -S chaotic-mirrorlist chaotic-keyring`

If not on ArcoLinux you can install them from AUR.


Install these two packages on your system if you want to include **ArcoLinux packages** on the iso

`sudo pacman -S arcolinux-mirrorlist-git arcolinux-keyring`

If not on ArcoLinux you can download the package from the alci_repo with sudo pacman -U.

https://github.com/arch-linux-calamares-installer/alci_repo


After editing the necessary files (pacman.conf and packages.x86_64) you can start building.

Use the scripts from this folder:

<b>installation-scripts</b>

Use script 30 and it will clean your pacman cache and redownload every package it needs.

Use script 40 to use your current pacman cache - it will only download what is needed.

You will find the iso in this folder:

 ~/Alci-Iso-Out

Burn it with etcher or other tools and use it.

Still not sure what to do.

Check out the playlist on Youtube

https://www.youtube.com/playlist?list=PLlloYVGq5pS4vhYQuLikS8dhDjk6xaiXH


# Installation process

Is documented on 

https://www.alci.online


# After installation

We have added a script to activate your display manager by default.
If you reboot you will boot into a graphical environment.

If you did not install a desktop environment on the iso you can still do so by going to 
TTY and installing one. SDDM stays after installation.

If you install more than one display manager they will overrule each other. SDDM will always lose
to gdm, lightdm or lxdm.


If you are still in the terminal then activate the display manager of your choice manually.

`sudo systemctl enable gdm`

`sudo systemctl enable lightdm`

`sudo systemctl enable sddm`

`sudo systemctl enable lxdm`

Get the pacman databases in

`sudo pacman -Sy`

or update immediately

`sudo pacman -Syyu`


# Tip

Sometimes a "proc" folder stays mounted.

Unmount it with this

sudo umount /home/{username}/...  use the TAB


# Tip

We have added a /etc/pacman-more.conf file to your future system.
That way we have the ArcoLinux repos and Chaotic repos if we do decide to install it after all.
Remember to install the mirror and keys.


# Tip

Run into issues - remove all packages manually with

`sudo pacman -Scc`

and ensure they are all gone.


# Tip

When testing out the ALCI in virtualbox, you can use the alias 
evb to enable and start virtualbox. As a result you can use your full resolution.



# Tip

When using gdm as display manager remember to delete the file /archiso/airootfs/etc/motd from your system. That files comes originally from Arch Linux.
To avoid waiting for every login and this nice look.
https://imgur.com/a/EvCN4pm


# Tip

Internet is NOT required for ALCI. Calamares is only using the internet to check where you live to put the red dot correctly on the world map (geoip). Calamares will **not download anything**. 

The list you created in the packages.x86_64 file will be installed on the iso and on your future system.

On demand of our users we have added 3 links to the archiso folder so that in the live environment they will have network manager.

/archiso/airootfs/etc/systemd/system/multi-user.target.wants/NetworkManager.service
/archiso/airootfs/etc/systemd/system/network-online.target.wants/NetworkManager-wait-online.service
/archiso/airootfs/etc/systemd/system/dbus-org.freedesktop.nm-dispatcher.service

If you do not use Networkmanager, you can delete them. You can also keep them as they are pointing to services you have not installed. The links will have no effect at all.

Remember there is still **nmtui** if the gui Networkmanager fails you in some way.

If you did NOT install it on the iso. These are the steps you can still do.

`setxkbmap be  - I will set my keyboard to azerty`


`sudo pacman -Sy - get the pacman databases in`


`sudo pacman -S networkmanager - installing the software`


`sudo systemctl enable NetworkManager - mind the capital letters`


`sudo systemctl start NetworkManager`


`nmtui`

Then connect to the wifi.

Then we restart Calamares.

`sudo calamares`
