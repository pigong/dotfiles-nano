# Useful Snippets for the configuration of Jetson Orin Nano

## recover L4T-README

```
sudo mount -o loop /opt/nvidia/l4t-usb-device-mode/filesystem.img /media/$USER/L4T-README
```

## log into wifi from shell

```
sudo nmcli device wifi connect 'SSID' password 'PASSWORD'
```

## USB-Camera cheese startup error (works well on host):

### ERROR: JPEG parameter struct mismatch: library thinks size is 584, caller expects 728

### working fix not understood

```
sudo rm /usr/lib/aarch64-linux-gnu/gstreamer-1.0/libgstnvvideo4linux2.so
sudo rm /usr/lib/aarch64-linux-gnu/gstreamer-1.0/libgstnvjpeg.so
rm ~/.cache/gstreamer-1.0/registry.aarch64.bin
```

makes a big difference. Note however that also the chees Version can cause
serious problems. The snap version seems to work much better.

## Flash "manually", i.e. with scripts

Possibly a way to manually flash a proper "super-mode" kernel manually.

```
sudo ./tools/kernel_flash/l4t_initrd_flash.sh --external-device mmcblk0p1 \
  -c tools/kernel_flash/flash_l4t_t234_nvme.xml -p "-c bootloader/generic/cfg/flash_t234_qspi.xml" \
  --showlogs --network usb0 jetson-orin-nano-devkit-super internal
```

### Adjust the external device:

- SD-Card: mmcblk0p1
- Nvme-Drive: nvme0n1 or nvme0np1, /dev/nvme0n1
  - PTUUID="1da58d90-2319-47e9-953c-97626dbe641c" PTTYPE="gpt"

There are 15 partitions on the drive: nvme0n1p1 -> nvme0n1p15
So maybe the correct entry for the external-device shoulb be nvme0n1p1

```
/dev/nvme0n1p1: UUID="8ab1d4ec-b7fe-4264-9c0c-daed07018e5d" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="APP" PARTUUID="ba1b90fe-ff84-4b75-b22f-ff9e3760b0f4"
/dev/nvme0n1p2: PARTLABEL="A_kernel" PARTUUID="1d56a140-4fc1-4f99-937d-a367c5ca1e21"
/dev/nvme0n1p3: PARTLABEL="A_kernel-dtb" PARTUUID="503302f6-3800-4ce5-8abd-0b6b8ad63b37"
/dev/nvme0n1p4: PARTLABEL="A_reserved_on_user" PARTUUID="705c93d1-f780-4e4b-96e9-795e93c95b3c"
/dev/nvme0n1p5: PARTLABEL="B_kernel" PARTUUID="78fbd2fd-828d-4d25-88cc-fc48e87a2706"
/dev/nvme0n1p6: PARTLABEL="B_kernel-dtb" PARTUUID="1a4724bd-0e71-4481-9876-2815eea0bd4b"
/dev/nvme0n1p7: PARTLABEL="B_reserved_on_user" PARTUUID="7a893c03-2e9e-4cc2-b210-00744f628f17"
/dev/nvme0n1p8: PARTLABEL="recovery" PARTUUID="130f957d-569b-47e1-b851-b5150153e351"
/dev/nvme0n1p9: PARTLABEL="recovery-dtb" PARTUUID="463ec12d-d957-47c3-91c8-786d6d629563"
/dev/nvme0n1p10: UUID="E3E7-0BD2" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="esp" PARTUUID="075d2919-45e4-451c-b32d-b4040f2c9057"
/dev/nvme0n1p11: PARTLABEL="recovery_alt" PARTUUID="52017de4-ea7d-4fbf-9902-cc0eb5115e42"
/dev/nvme0n1p12: PARTLABEL="recovery-dtb_alt" PARTUUID="3e0be1fd-ebb0-4d45-88db-b97efbb40737"
/dev/nvme0n1p13: PARTLABEL="esp_alt" PARTUUID="1a6b6e3d-a751-47b6-a32f-2f3dfa92b234"
/dev/nvme0n1p14: PARTLABEL="UDA" PARTUUID="6c37b5c2-a6bc-4257-a933-7000c5f1c066"
/dev/nvme0n1p15: PARTLABEL="reserved" PARTUUID="4f19d55a-445b-4470-9454-507ed86a2962"
```

### fstab

```
# <file system> <mount point>             <type>          <options>                               <dump> <pass>
/dev/root            /                     ext4           defaults                                     0 1
UUID=E3E7-0BD2 /boot/efi vfat defaults 0 1
```
In /dev/disk/by-uuid only disk

```
8ab1d4ec-b7fe-4264-9c0c-daed07018e5d  E3E7-0BD2
```

is listed.


