# Creation and index of Born2beRoot - 42cursus

When a command has to be executed with sudo and you dont have it installed , you can simply change your shell with

	su

and then execute whatever command you want.


* # LVM

	## Concepts

   LVM stands for Logical Volume Management. It is a system of managing logical volumes, or filesystems, that is much more advanced and flexible than the traditional method of partitioning a disk into one or more segments and formatting that partition with a filesystem. 

   There are 3 concepts that LVM manages:

    - Volume Groups
		
	    A Volume Group is a named collection of physical and logical volumes. Typical systems only need one Volume Group to contain all of the physical and logical volumes on the system, and I like to name mine after the name of the machine. 

    - Physical Volumes

		Physical Volumes correspond to disks; they are block devices that provide the space to store logical volumes. 

	- Logical Volumes

		Logical volumes correspond to partitions: they hold a filesystem. Unlike partitions though, logical volumes get names rather than numbers, they can span across multiple disks, and do not have to be physically contiguous. 

	From [Ubuntu Wiki](https://wiki.ubuntu.com/Lvm)

	## What is LVM and what is it used for?

	You can think of LVM as "dynamic partitions", meaning that you can create/resize/delete LVM "partitions" (they're called "Logical Volumes" in LVM-speak) from the command line while your Linux system is running: no need to reboot the system to make the kernel aware of the newly-created or resized partitions.

	Other nice features that LVM "Logical Volumes" provide are:

    1. If you have more than one hard-disk, Logical Volumes can extend over more than one disk: i.e., they are not limited by the size of one single disk, rather by the total aggregate size.

    2. You can set up "striped" LVs, so that I/O can be distributed to all disks hosting the LV in parallel. (Similar to RAID-0, but a bit easier to set-up.)

    3. You can create a (read-only) snapshot of any LV. You can revert the original LV to the snapshot at a later time, or delete the snapshot if you no longer need it. This is handy for server backups for instance (you cannot stop all your applications from writing, so you create a snapshot and backup the snapshot LV), but can also be used to provide a "safety net" before a critical system upgrade (clone the root partition, upgrade, revert if something went wrong).

	From [AskUbuntu](https://askubuntu.com/questions/3596/what-is-lvm-and-what-is-it-used-for)

	## How-To

	Install with:
		
		sudo apt-get install lvm2

	First, you need a Physical Volume. Typically you start with a hard disk, and create an LVM type partition on it. You can create one with gparted or fdisk, and usually only want one partition to use the whole disk, since LVM will handle subdividing it into Logical Volumes. In gparted, you need to check the lvm flag when creating the partition, and with fdisk, tag the type with code 8e.

	Once you have your LVM partition, you need to initialize it as a Physical Volume. Assuming this partition is /dev/sda1:

		sudo pvcreate /dev/sda1
	
	This writes the LVM header to the partition, which identifies it as a Physical Volume, and sets up a small area to hold the metadata describing everything about the Volume Group, and the the rest of the partition as unused Physical Extents. After that, you need to create a Volume Group named foo:

		sudo vgcreate foo /dev/sda1

	Now you have a Volume Group named foo. I suggest you change foo to a name meaningful to you. foo contains only one Physical Volume. Now you want to create a Logical Volume from some of the free space in foo:

		sudo lvcreate -n bar -L 5g foo  

	This creates a Logical Volume named bar in Volume Group foo using 5 GB of space. If you are installing, you probably want to create a Logical Volume like this to use as a root filesystem, and one for swap, and maybe one for /home. I currently have a Logical Volume for a Lucid install, and one for a Maverick install, so that is what I named those volumes. You can find the block device for this Logical Volume in '/dev/foo/bar' or 'dev/mapper/foo-bar'. 

	...

	From [Ubuntu Wiki,](https://wiki.ubuntu.com/Lvm) also see [Debian Wiki](https://wiki.debian.org/LVM)

* # AppArmor
  	
	From [Debian-Handbook](https://debian-handbook.info/browse/en-US/stable/sect.apparmor.html)
	
	## Concepts

	AppArmor is a Mandatory Access Control based un Linux Security Modules. Basically it confines resources for certain applications in order to make them not able to access other application's resources.

	AppArmor applies a set of rules, _a profile_ to some program. What profile is used depends on where the program was installed, _its installation path_.

	AppArmor's profiles are stored in **`/etc/apparmor.d/`**

	AppArmor's rules can be set to two modes:
  
	- Enforce: The rules are applied as it registers violation tries.
	- Complain: The rules are **not** applied but it registers violation tries. 

	Install with

		sudo apt install apparmor apparmor-profiles apparmor-utils
	
	## How-To

* # Path

	## How To
	Let’s say you have a directory called bin located in your Home directory in which you keep your shell scripts. To add the directory to your $PATH type in:

		export PATH="$HOME/bin:$PATH"

	The export command will export the modified variable to the shell child process environments.
	
	From [linuxize](https://linuxize.com/post/how-to-add-directory-to-path-in-linux/)

* # Apt-get vs Apt vs Apritude
* https://juncotic.com/apt-vs-apt-get-vs-aptitude-algunas-notas/
* https://debian-handbook.info/browse/es-ES/stable/sect.apt-get.html