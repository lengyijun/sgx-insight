# /dev/isgx

/dev/isgx 是一个设备文件，是对sgx的抽象。是sgx环境中必不可少的。

要与设备文件通信，使用ioctl函数，可以`man ioctl`

```
IOCTL(3P)                                    POSIX Programmer's Manual                                    IOCTL(3P)

PROLOG
       This  manual  page is part of the POSIX Programmer's Manual.  The Linux implementation of this interface may
       differ (consult the corresponding Linux manual page for details of Linux behavior), or the interface may not
       be implemented on Linux.

NAME
       ioctl — control a STREAMS device (STREAMS)

SYNOPSIS
       #include <stropts.h>

       int ioctl(int fildes, int request, ... /* arg */);

```

具体使用是这样的

```c
int fd=fopen("/dev/isgx",...);
ioctl(fd,....);
```

要产生 `/dev/isgx` 这个文件，需要编译 [linux-sgx-driver](https://github.com/intel/linux-sgx-driver) 

linux-sgx-driver 本质上就是响应 ioctl 的handler. 注意发起ioctl只能从非安全区发起，不能从enclave里面发起。

典型的ioctl有：

- 创建enclave
- 初始化enclave

还有些不是以ioctl为入口，但是也是实现在 linux-sgx-driver中的，比如：

- EPC空闲页的管理
- EPC的swap in/out
- EPC的page fault handler