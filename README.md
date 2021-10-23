# mount.part
Mount a single partition from a whole disk image file

```
USAGE
    mount.part diskimage mountpoint [options...]

DESCRIPTION
    mount.part mounts a partition found in the given image file diskimage
    at the specified path mountpoint using the default loop device.

    The partition to mount is selected via interactive prompt, and its
    file offset is computed automatically by parsing fdisk(8) output.

    diskimage must be a path to a whole disk image (e.g., .img, .iso).
    mountpoint is created if it does not exist. Remaining options are
    passed verbatim to mount(8).

NOTES
    Requires sudo permissions for mount(8).
```

## Examples

### Multiple partitions in disk image
If multiple partitions are found in the given disk image, each partition is printed and nummbered, and a prompt is presented to select which partition to mount:

```sh
$ mount.part backup-disk.iso ~/.mnt/iso
Partitions found:

        [1] backup-disk.iso1       8192     15486975 15478784 7.4G  c W95 FAT32 (LBA)
                offset: 512 * 8192 = 4194304
                
        [2] backup-disk.iso2       15486976 30965759 15478784 7.4G  c W95 FAT32 (LBA)
                offset: 512 * 15486976 = 7929331712

Select partition: [#] 2
$
```

### Single partition in disk image
If only a single partition is found, it is automatically selected and the user is prompted to proceed with mounting it:

```sh
$ mount.part Windows.iso ~/.mnt/iso
Partitions found:

        [1] Windows.iso1       8192 30965759 30957568 14.8G  c W95 FAT32 (LBA)
                offset: 512 * 8192 = 4194304

Selected partition: 1
Continue? [Y/n] y
$
```
