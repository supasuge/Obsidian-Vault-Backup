## Table of Contents

- [Getting Started](#getting\started)
    - [Sections of a Disk Image](#Sections\of\a\Disk\Image)
          - [Note: NOT COMPLETE](#Note:\NOT\COMPLETE)

Forensics is a complex topic in modern technology, this will only be an introduction to some of the tools used. It's encouraged and highly reccomended to do further research on these tools, and the different use cases and capabailities. 


# Getting Started
Firstly... Let's download the file! `wget` or `curl` the file to your directory of choice.

Now let's get some more information about the file:
```bash
file <FILE_NAME>
...
```

### Sections of a Disk Image
	Four Main Layers
- **Media**: The media layer tools are all prepended with 'mm' and operate on the disk image with little guidance from the analyst. `mmls` is a media layer tool that gives us the partition table of the image and key information for delving into the other layers. Media is the lowest level, providing key inforation to access the deeper layers, but not shedding much light on the data contained in the image
- **Block**: The block layer is the second lowest level of the four layers considered here. Block layer tools are prepended with `blk` in the SleuthKit. `blkcat` is a block layer tool that outputs the contents of a single block. The block layer is the numbers of the disk image broken into equal-sized chunks.A single file is likely to contain multiple blocks
- **Inode**: The inode layer is the bookkeeping layer of the disk image. It's like the table of contents, with the chapter numbers being like the inodes and the pages like the blocks of a file. Inode layer tools are prepended with `i`. `icat` is inode layer toolf that output a single file base on its inode number


###### Note: NOT COMPLETE