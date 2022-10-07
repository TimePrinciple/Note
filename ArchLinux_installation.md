Make sure booted from UEFI mode.

Check for internet connection is up.

    For WiFi, type `iwctl` to go into the WiFi manager.

    Then `device list`, choose the WiFi card and `station <wifi-card> scan`, `station <wifi-card> get-networks`, `station <wifi-card> connect <selected-wifi>`, put in the password if required.

Ping archlinux.org to make sure the connect is up.

For direct connection ...

Sync the date and time.

    Type `timedatectl set-ntp true`.

    Type `timedatectl status` to check whether NTP is actived.

Partition the disks.

    Type `fdisk -l` to find the storage.

    `fdisk /dev/<storage device>` to start partitioning.

    `g` for GPT table.

    `n` for new partition.

    Normally, three partitions are required during the installation:

        A boot partition, 512M large, with the type of EFI system.

        A root partition, 20G large, with the type of Linux root (\<architecture\>).

        A home partition, takes up the rest of the device by  default, with the type of Linux home.

        A swap partition, takes up the size of RAM plus the square root of RAM, with the type of Linux swap.

Type `w` to write the changes.

Format the disks just partitioned.

    Type `mkfs.vfat /dev/<boot partition>`.

    Type `mkfs.ext4 /dev/<root partition>`.

    Type `mkfs.ext4 /dev/<home partition>`.

    Type `mkswap /dev/<swap partition>`.

Mount filesystems.

    Type `mount /dev/<root partition> /mnt` to mount root to its partition.

    Create the mount point of boot and home with `mkdir /mnt/boot` and `mkdir /mnt/home`

    Type `mount /dev/<boot partition> /mnt/boot` to mount boot partition.

    Type `mount /dev/<home partition> /mnt/home` to mount home partition.

    Type `swapon /dev/<swap partition>`.

    Type `df` to make sure everything is in its place.

Install components to root.

    Type `pacstrap /mnt base base-devel linux linux-firmware networkmanager dhcpcd vim` to install necessary packages.

Generate fstab.

    Type `genfstab -U /mnt >> /mnt/etc/fstab`

Change root to /mnt.

    Type `arch-chroot /mnt`

Set time zone.

    Type `ln -sf /usr/share/zoneinfo/Hongkong /etc/localtime`

    Type `hwclock --systohc`

Set localization.

    Type `vim /etc/locale.gen` to edit and uncomment `en_US.UTF-8 UFT-8` and `zh_CN.UTF-8 UTF-8`.

    Then type `locale-gen`.

    Type `vim /etc/locale.conf` to add `LANG=en_US.UTF-8`.

Network configuration.

    Type `vim /etc/hostname` to add `<hostname>`.

    Type `vim /etc/hosts` to add:

```
127.0.0.1    localhost
::1    localhost
127.0.1.1    <hostname>.localdomain    <hostname>
```

    Type `passwd` to change password for root.

Install bootloader.

    Type `pacman -S grub efibootmgr os-prober`.

    Type `grub-install --target=x86_64-efi --efi-directory=/boot/ --bootloader-id=GRUB`

    Type `grub-mkconfig -o /boot/grub/grub.cfg`

    Type `exit` and then reboot.

After reboot.

    Login with root and the password just set.

    Type `systemctl start NetworkManager` and `systemctl enable NetworkManager`.

    `nmtui` can be used to connect to Wi-Fi.

Add a user.

    Type `useradd -m -g wheel <user name>`.

    Type `passwd <user name>`.

    Type `pacman -S vi sudo`.

    Type `visudo` to uncomment settings around `wheel` group. e.g. `%wheel ALL=(ALL) ALL`.

Install utilities.

    Type `pacman -S xorg-server xorg-xinit`.

    Type `pacman -S lightdm`.

    Type `pacman -S lightdm-gtk-greeter`.

    Type `pacman -S lightdm-gtk-greeter-settings`.

    Type `systemctl enable lightdm`.

    Type `pacman -S i3-gaps i3status i3lock dmenu`.

    Type `pacman -S xfce4` if necessary.

    Type `pacman -S alacritty`.

    Type `pacman -S firefox`.

    Type `pacman -S nautilus`.

    Type `pacman -S ttf-dejavu wqy-microhei`

