## Download

Download ISO from here: https://archlinux.org/download/

## Write to USB

### MacOS

```
diskutil list
diskutil unmountDisk /dev/diskX
dd if=archlinux-2021.06.01-x86_64.iso of=/dev/rdiskX bs=1m
```
