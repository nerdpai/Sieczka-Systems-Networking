# nfs hw

## plan

- [X] 1. Divide the disk into two partitions sdb1 and sdb2.

- [X] 2. On the system, increase the partitions
"/" by the disk space of sdb1 using LVM

- [X] 3. On sdb2, create an ext4 file system and mount
the disk space in the "/data" directory

- [X] 4. Then, using NFS, make the "/data"
directory available on the network for machine2

- [X] 5. On machine2, mount the "/data"
resource in the "/shared" directory from machine1

## nfs server

### ATTENTION

***ASSUMPTION:*** root is logical volume.

### 1. Divide the disk into two partitions sdb1 and sdb2

1. `lsblk -d -o NAME,SIZE | grep '^sd'` — show the list of disks.
1. `sudo cfdisk <name_of_the_disk>` — start manage space.
    1. select `gpt`.
    1. choose `free space`.
    1. enter the `size` for the partition.
    1. for the second partition do the same.
    1. `Write` first and second partition.
    1. `Quit` to exit.
1. `sudo fdisk -l <name_of_the_disk>` — list newly created partitions.

### 2. Increase space of root partition

1. `df -h /` — which partition is mounted as root.
1. `sudo pvcreate <partition_name>` — create a physical volume
1. `sudo vgextend <volume_group_name> <partition_name>`
— to extend the volume group by partition.
1. `pvs -o +vg_name | grep <volume_group_name>`
— display the phisical volumes of the vg.
1. `sudo lvextend -rl +100%FREE <logical_volume_name>`
— increase space of the lv.

### 3. Manage second partition

1. `cd /` — go to the root.
1. `sudo mkdir data` — create a directory 'data'.
1. `sudo mkfs.ext4 <partition_name>` — create a ext4 FS in the partition.
1. `sudo mount <partition_name> /data` — mount the partition space in the "/data".

### 4. NFS server

Install server — `sudo apt install nfs-kernel-server`.
`sudo systemctl enable --now nfs-kernel-server` — tunr it on.

`sudo chmod 775 <directory_to_share>` — make share directory accessable.

`sudo nano /etc/exports`
and then add this line:`<directory_to_share> <client_ip>([options])`.
E.g `/ 22.22.22.21(rw,sync,no_subtree_check)`

`sudo systemctl restart nfs-kernel-server` after changes.
And we should have possibility to connect to.

## nfs client

### 5. NFS client

Install client — `sudo apt install nfs-common`.

`sudo mkdir /shared` — create a directory 'shared'.

`sudo mount -t nfs <host_ip>:<directory_to_share> /shared`
— to mount.
