# Mounting Stuff

*This is just a bunch of disjointed notes with various mount commands I use that I am putting here to make it easier for me to find them in the future. Nothing too fancy to see here. If you find it helpful, great!*

## Mount Windows Share

What seems to be missing from online examples that I seem to always need is `vers=2.0`. This isn't needed in many cases, so I usually try it without and then try again with it if it failed. The password can also be provided in the command by adding `password=PASSWORD` but I'm not generally a fan of putting passwords in my bash history so I prefer to let it prompt me.

    # mount -t cifs -o domain=DOMAIN,username=USERNAME,vers=2.0 //HOSTNAME/shared_folder /mnt/target

## Mount LVM Volumes from VMDK Files

Create a mount point, loopback device, and view partitions present:

    # mkdir /mnt/target
    # losetup /dev/loop0 mysystem-flat.vmdk
    # fdisk -l /dev/loop0

Expected output should be similar to the following:

    Disk /dev/loop0: 20 GiB, 21474836480 bytes, 41943040 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: dos
    Disk identifier: 0x000507d4
    
    Device       Boot   Start      End  Sectors  Size Id Type
    /dev/loop0p1 *       2048  1026047  1024000  500M 83 Linux
    /dev/loop0p2      1026048 41943039 40916992 19.5G 8e Linux LVM

Next calculate the offsets in bytes instead of sectors by multiplying the starting sectors by the sector size. Then create a loopback device from each offset:    

    # echo "2048*512" | bc
    1048576

    # losetup -o 1048576 /dev/loop1 mysystem-flat.vmdk

    # echo "102648*512" | bc
    525336576

    # losetup -o 525336576 /dev/loop2 mysystem-flat.vmdk

Then scan all devices for volume groups:

    # vgscan
      Reading all physical volumes.  This may take a while...
      Found volume group "centos" using metadata type lvm2

Next get information on physical and logical volumes:

    # pvs
      PV         VG     Fmt  Attr PSize  PFree 
      /dev/loop2 centos lvm2 a--  19.51g 40.00m

    # lvs
      LV   VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
      root centos -wi-a----- 17.47g                                                    
      swap centos -wi-a-----  2.00g

Mount the logical volumes. Sometimes the home folder is in a separate vmdk file and needs to be mounted in its appropriate location under the main volume:

    # mount -o ro,norecovery /dev/centos/root /mnt/target
    # ls /mnt/target
    bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
    boot  etc  lib   media  opt  root  sbin  sys  usr

**Dismounting LVM Volume(s)**

To dismount the volumes, do the following (keeping in mind you may need to first dismount "sub-mounted" volumes such as the home folder or other paths from separate vmdk files, etc.):

    # umount /mnt/target
    # vgchange -a n centos
      0 logical volume(s) in volume group "centos" now active

    # losetup -D

------------------------------

### Credits

Many thanks to [Steve Gibson](https://about.me/sgibson) for making a cheat sheet for mounting LVM volumes that I've more or less just reproduced here.

