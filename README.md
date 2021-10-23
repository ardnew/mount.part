# mount.part
Mount a single partition from a whole disk image

```
USAGE
    mount.part disk-image mount-point [options...]

DESCRIPTION
    mount.part mounts a partition found in the given image file disk-image
    at the specified path mount-point using the default loop device.

    The partition to mount is selected via interactive prompt.

    disk-image must be a path to a whole disk image (e.g., .img, .iso).
    mount-point is created if it does not exist. Remaining options are
    passed verbatim to mount(8).

NOTES
    Requires sudo permissions for mount(8).
```
