## Table of Contents

  - [Table of Contents](#Table\of\Contents)

## Table of Contents


Installing a ISO image to a drive
```bash
lsblk #find device

sudo umount /dev/sda #make sure the device is unmounted

sudo dd if=/path/to/image.iso of=/dev/sda bs=4M conv=fdatasync status=progress
#load img to disk
```



