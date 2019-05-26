# Logical Volume Manager

![Alt Text](http://www.howtogeek.com/wp-content/uploads/2011/02/652x202xbanner-1.png.pagespeed.gp+jp+jw+pj+js+rj+rp+rw+ri+cp+md.ic.VGSxDeVS9P.png)


## LVM install

```bash
apt-get install lvm2
```

## LVM cheatsheet

  http://www.howtogeek.com/wp-content/uploads/2011/01/lvm-cheatsheet.png


## Basic use case

### create physical volume

```bash
pvcreate /dev/sdb1
pvcreate /dev/sdb2

pvdisplay
pvdisplay /dev/sdb1

pvs -v
```

### create volume group using 2 physical volumes

```bash
vgcreate vg01 /dev/sdb1 /dev/sdb2

vgdisplay vg01
ls -l /dev/vg01

vgs -v
```


### create a logical volume

=> /dev/vg01/lvol1

```bash
lvcreate -n lvol1 -L 130M vg01

lvdisplay
lvdisplay /dev/vg01/lvol1

lvs -v
lvscan

mkfs -t ext4 /dev/vg01/lvol1
mkdir /u02
mount /dev/vg01/lvol1 /u02

```

> lvol1 is mounted as /dev/mapper/vg01-lvol1


## Linux partitionning

 * LV home
 * LV root
 * LV_swap_1
 * ext2 / boot
 

```bash

pvcreate /dev/sda1
vgcreate vg-sys /dev/sda1

lvcreate -L 1G -n boot vg-sys
lvcreate -L 8G -n swap vg-sys
lvcreate -l 100%FREE -n root vg-sys

pvcreate /dev/sda2
vgcreate vg-user /dev/sda2
lvcreate -l 100%FREE -n user vg-user

mkswap -L swap /dev/vg-sys/swap
mkfs -t ext4 -L root /dev/vg-sys/boot
mkfs -t ext4 -L root /dev/vg-sys/root
mkfs -t ext4 -L user /dev/vg-user/user

```