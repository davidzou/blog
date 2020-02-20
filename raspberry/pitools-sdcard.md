# 树莓派SDCard烧录 ------ 命令行

> 新TF卡显示的内容。

* diskutil list 显示内容

	```
	/dev/disk2
	   #:                       TYPE NAME                    SIZE       IDENTIFIER
	   0:     FDisk_partition_scheme                        *63.9 GB    disk2
	   1:               Windows_NTFS                         63.8 GB    disk2s1
	```

* diskutil info /dev/disk2 显示内容

	```

	   Device Identifier:        disk2
	   Device Node:              /dev/disk2
	   Part of Whole:            disk2
	   Device / Media Name:      Mass Storage Device Media

	   Volume Name:              Not applicable (no file system)

	   Mounted:                  Not applicable (no file system)

	   File System:              None

	   Content (IOContent):      FDisk_partition_scheme
	   OS Can Be Installed:      No
	   Media Type:               Generic
	   Protocol:                 USB
	   SMART Status:             Not Supported

	   Total Size:               63.9 GB (63864569856 Bytes) (exactly 124735488 512-Byte-Units)
	   Volume Free Space:        Not applicable (no file system)
	   Device Block Size:        512 Bytes

	   Read-Only Media:          No
	   Read-Only Volume:         Not applicable (no file system)
	   Ejectable:                Yes

	   Whole:                    Yes
	   Internal:                 No
	   OS 9 Drivers:             No
	   Low Level Format:         Not supported

	```

> 格式化TF卡

```
zzw:davidzou$ sudo diskutil eraseDisk FAT32 ZZWZM MBRFormat /dev/disk2
Password:
Started erase on disk2
Unmounting disk
Creating the partition map
Waiting for the disks to reappear
Formatting disk2s1 as MS-DOS (FAT32) with name ZZWZM
512 bytes per physical sector
/dev/rdisk2s1: 124704960 sectors in 1948515 FAT32 clusters (32768 bytes/cluster)
bps=512 spc=64 res=32 nft=2 mid=0xf8 spt=32 hds=255 hid=2 drv=0x80 bsec=124735486 bspf=15223 rdcl=2 infs=1 bkbs=6
Mounting disk
Finished erase on disk2

```
> 格式化后

* diskutil list

	```
	/dev/disk2
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *63.9 GB    disk2
   1:                 DOS_FAT_32 ZZWZM                   63.9 GB    disk2s1
	```

* diskutil info /dev/disk2

	```
	   Device Identifier:        disk2
   Device Node:              /dev/disk2
   Part of Whole:            disk2
   Device / Media Name:      Mass Storage Device Media

   Volume Name:              Not applicable (no file system)

   Mounted:                  Not applicable (no file system)

   File System:              None

   Content (IOContent):      FDisk_partition_scheme
   OS Can Be Installed:      No
   Media Type:               Generic
   Protocol:                 USB
   SMART Status:             Not Supported

   Total Size:               63.9 GB (63864569856 Bytes) (exactly 124735488 512-Byte-Units)
   Volume Free Space:        Not applicable (no file system)
   Device Block Size:        512 Bytes

   Read-Only Media:          No
   Read-Only Volume:         Not applicable (no file system)
   Ejectable:                Yes

   Whole:                    Yes
   Internal:                 No
   OS 9 Drivers:             No
   Low Level Format:         Not supported

	```
> 退出挂载

	```
	diskutil umount /dev/disk2s1
	```
> 烧录镜像

	```
	sudo dd if=2017-04-10-raspbian-jessie.img of=/dev/disk2 bs=4m
	```
