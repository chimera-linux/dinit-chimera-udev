# dinit-chimera-udev

This is udev integration for [dinit-chimera](https://github.com/chimera-linux/dinit-chimera).

It provides a udev-based device monitor.

Currently, it is possible to depend on individual devices (`/dev/foo`), on
`/sys` paths, on network interfaces, on MAC addresses, and on USB
`vendor:product` strings.

For devices, it just looks like `/dev/foo`, for `/sys` paths it's a long native
path like `/sys/devices/...`, for network interfaces it's `netif:foo`, for MAC
addresses it's `mac:foo` (the address must be in lowercase format), for USB
IDs it's `usb:vendor:product` with lowercase hex (e.g. `usb:1d6b:0003`).
Additionally, disk aliases are supported, e.g. `device@PARTLABEL=foo` is equal
to `device@/dev/disk/by-partlabel/foo`.

For non-USB devices, they may appear and disappear according to their syspath.
For USB devices, which cannot be matched accurately by a syspath as you may have
multiple devices with the same vendor/product ID pair in your system, they
appear with the first device and disappear with the last device.

Devices from the `block`, `net`, `tty`, and `usb` subsystems are matched
automatically.
If you wish to match devices from other subsystems, they have to carry
the tag `dinit` or `systemd` (for compatibility).

Example service for `netif` type:

```
type = process
command = /usr/bin/foo
depends-on: local.target
depends-ms: device@netif:wlp170s0
```

It is also possible to create soft dependencies of the device services on
other services from within `udev` rules. To do this, the `DINIT_WAITS_FOR`
property can be used and the `dinit` tag must exist on the device. Like so:

```
TAG+="dinit", ENV{DINIT_WAITS_FOR}+="svc1 svc2"
```

Any service that depends on a device service belonging to the above will
be held back until the specified services have started or failed to start.
