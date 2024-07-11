[Linux Syscalls](https://x64.syscall.sh/)
### read
```c
int read(int fd, void *buf, size_t count)
```


### write
```c
int write(int fd, void *buf, size_t count)
```
- Writes up to count bytes from the buffer starting at buf to the filer referred to by the file descriptor fd

### open
```c
int open(char *pathname, int flags, mode_t mode)
```
- The open() system calls opens the files specified by pathname. If the specified file does not exist, it may optionally (if O_CREAT) is specified in flags be created by open()
	- The return val of open() is a file descriptor (fd) that is used in subsequent systems calls (read(2), write(2), lseek(2), fcntl(2).) To refer to the open file. The descriptor returned by a successful call will be the lowest-number file descriptor not currently open for the process.


## Linux Processes
