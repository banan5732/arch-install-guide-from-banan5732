#hello, this is a small guide from me on how to install arch linux

first what we do is running "lsblk" to see all our drive, you will look for which you want install your arch linux, i will use /dev/sda

second, we create partitions, delete all partitions that exist and you will see "free space"

i highly recomend you to create 3 partitions, first for boot (800M), second for swap (2-3GB) and the root for all space that will be free

than you format the partitions into their filesystems

do this command

mkfs.fat -F 32 /dev/sda1
mkswap /dev/sda2
mkfs.ext4 /dev/sda3

now we mount these partitions into their right place

mount /dev/sda3 /mnt                    <- this is our root partition where all system files are located
swapon /dev/sda2                        <- this is our swap partition, basically just extra ram for you PC
mount --mkdir /dev/sda1 /mnt/boot       <- this is our boot partition where boot files are located

after we mounted our partitions we install minimal system with "pacstrap"

pacstrap -K /mnt base linux linux-firmware grub efibootmgr networkmanager sudo    <- these are our must-have packages

and also you can install some others programs like "vim" "nano" "firefox" for it you just add them to your pacstrap packages


after we generate fstab with

genfstab -U /mnt >> /mnt/etc/fstab


now you chroot into your system by

arch-chroot /mnt


in chroot you set up you system

for time you do

ln -sf /usr/share/zoneinfo/Region/City /etc/localtime

change "zoneinfo/Region/City" to your Region and city

and now you do

hwclock --systohc #to generate /etc/adjtime


to add localization you do is edit /etc/locale.gen with your text editor, if you don't have one install it with "pacman -S 'text editor'" for this i use nano
after you installed text editor go into /etc/locale.gen by

nano /etc/locale.gen

but also you can add stuff by typing echo 'text' >> /etc/locale.gen and add there "LANG=en_US.UTF-8" for english

now you type

locale-gen

to generate locales

and after you generated locales add "LANG=en_US.UTF-8" to /etc/locale.conf by

echo LANG=en_US.UTF-8 >> /etc/locale.conf

now you create your hostname by

echo "your hostname" >> /etc/hostname

this will be the text that you'll see in system terminal if you use bash
example: user@'hostname'


after all you create your user by

useradd "username"

and adding him to wheel group by

usermod -aG wheel <username>

and adding a password by passwd <username> 'password you want'

and also add password to root by same but changing username to root

and add permission to use "sudo" for wheel group editing visudo by

EDITOR=nano visudo
where you find line "## Uncomment to allow members of group wheel to execute any command
 #wheel ALL=(ALL:ALL) ALL""

 just delete "#" to add permission

 NOW one of the most important stage -> installing bootloader

 grub-install --target=x86_64-efi --efi-directory=/mnt/boot --bootloader-id=GRUB


now install DE (i recomend gnome for begginers)
pacman -S gnome

now run

systemctl enable NetworkManager gdm3

 now exit chroot by
 exit


 reboot and BOOM youre done!



 P.S (sorry for spelling mistakes, english is not my main lang)
