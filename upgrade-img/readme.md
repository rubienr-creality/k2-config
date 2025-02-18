# Extract K2 Configuration from Update Image

1. copy update-image from printer
2. unpack initramfs
3. mount root (squashfs) partition
4. find K1 Plus configuration

## Copy update-image form printer

- image path on device (where to extract orig. congfigs from): `/mnt/UDISK/creality/upgrade/`
- config path on device (ev. where to restore configs): `/mnt/UDISK/printer_data/config/`

```bash
mkdir foo && cd foo

scp -r -O root@<yourk2printer>:/mnt/UDISK/creality/upgrade .

ls
upgrade/CR0CN240110C10_ota_img_V1.1.2.6.img # at the time of wriring, latest version was 1.1.2.6
```

## Unpack initramfs
```bash
lsinitramfs -l upgrade/CR0CN240110C10_ota_img_V1.1.2.6.img
-rwxrwxr-x   1 1001     sbuild       3215 Jan 21 14:24 sw-description
-rw-rw-r--   1 1001     sbuild    1294336 Jan 21 14:24 uboot
-rw-rw-r--   1 1001     sbuild    5423104 Jan 21 14:24 kernel
-rw-r--r--   1 1001     sbuild   121634816 Jan 21 14:24 rootfs
-rw-rw-r--   1 1001     sbuild        171 Jan 21 14:24 cpio_item_md5
cpio: premature end of archive

mkdir initramfs
unmkinitramfs upgrade/CR0CN240110C10_ota_img_V1.1.2.6.img initramfs

tree -L 2 initramfs
initramfs
├── early
│   ├── cpio_item_md5
│   ├── kernel
│   ├── rootfs
│   ├── sw-description
│   └── uboot
└── main
```

## Mount root (squashfs) partition

```bash
mkdir root-squashfs
sudo mount initramfs/early/rootfs root-squashfs

tree -L 1 root-squashfs
root-squashfs
├── bin
├── dev
├── etc
├── init
├── lib
├── mnt
├── overlay
├── proc
├── rdinit
├── rom
├── root
├── sbin
├── sys
├── tmp
├── usr
├── var -> tmp
└── www
```

## Find K1 Plus configuration

```bash
# find coonfigs in:
root-squashfs/usr/share/klipper/config/

# at the time of writing, similar configs to K2 Plus original configs were found in
ls root-squashfs/usr/share/klipper/config/F008_CR0CN240319C13
box.cfg              gcode_macro.cfg  printer_params.cfg
factory_printer.cfg  printer.cfg      sensorless.cfg

```
