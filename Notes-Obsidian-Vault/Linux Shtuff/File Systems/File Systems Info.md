1. `ext4` (Fourth Extended File System):
    - `ext4` is an extension of the `ext3` file system and is widely used as the default file system in many Linux distributions.
    - It supports file systems up to 1 exabyte in size and individual files up to 16 terabytes.
    - `ext4` uses a traditional inode-based structure, where each file and directory is represented by an inode that contains metadata and pointers to data blocks.
    - It includes features like extents (contiguous blocks of data), delayed allocation (optimizing block allocation), and journal checksum (ensuring data integrity).
    - `ext4` is known for its stability, performance, and compatibility with older `ext` file systems.
2. `btrfs` (B-tree File System):
    - `btrfs` is a copy-on-write (COW) file system that focuses on advanced features and scalability.
    - It uses a B-tree data structure to organize the file system, allowing for efficient access and modification of data.
    - `btrfs` supports features like snapshots (point-in-time copies of the file system), subvolumes (isolated file system sections), and transparent compression.
    - It provides built-in support for RAID (Redundant Array of Independent Disks) and allows for online resizing and defragmentation.
    - `btrfs` aims to provide a modern, feature-rich file system with enhanced data integrity and management capabilities.
3. `XFS` (eXtended File System):
    - `XFS` is a high-performance file system designed for scalability and large file systems.
    - It uses a B+ tree structure for efficient indexing and supports file systems up to 8 exabytes in size.
    - `XFS` employs an allocation group mechanism to distribute metadata and data across the file system, improving parallel I/O performance.
    - It includes features like delayed allocation, extended attributes, and online defragmentation.
    - `XFS` is known for its robustness, scalability, and ability to handle large files and file systems efficiently.
4. `ZFS` (Zettabyte File System):
    - `ZFS` is a advanced file system and logical volume manager originally developed by Sun Microsystems.
    - It uses a copy-on-write mechanism and supports features like snapshots, clones, and data integrity checks.
    - `ZFS` provides built-in support for RAID, allowing for data redundancy and protection against disk failures.
    - It offers advanced features like dynamic striping, compression, deduplication, and automatic repair of data corruption.
    - `ZFS` focuses on data integrity, scalability, and ease of administration, making it popular in enterprise and storage environments.
5. `F2FS` (Flash-Friendly File System):
    - `F2FS` is a file system optimized for NAND flash memory, commonly used in solid-state drives (SSDs) and mobile devices.
    - It is designed to address the unique characteristics of flash memory, such as limited write endurance and performance.
    - `F2FS` employs a log-structured approach, where data is written sequentially to minimize write amplification and improve write performance.
    - It supports features like inline data compression, encryption, and garbage collection to optimize flash memory usage.
    - `F2FS` aims to provide high performance and efficient utilization of flash storage devices.