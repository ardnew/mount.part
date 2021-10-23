# mount.part
Mount a single partition from a whole disk image

```
USAGE
    mount.part diskimage mountpoint [options...]

DESCRIPTION
    mount.part mounts a partition found in the given image file diskimage
    at the specified path mountpoint using the default loop device.

    The partition to mount is selected via interactive prompt.

    diskimage must be a path to a whole disk image (e.g., .img, .iso).
    mountpoint is created if it does not exist. Remaining options are
    passed verbatim to mount(8).

NOTES
    Requires sudo permissions for mount(8).
```
