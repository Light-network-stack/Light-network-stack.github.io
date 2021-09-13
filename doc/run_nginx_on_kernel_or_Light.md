# Run Nginx on Kernel or Light Stack

## 1. Set the size of webpage file

``` shell
sudo ./set_webpage_size.sh <file_size_in_byte>
```

## 2. Run nginx on either Kernel stack or Light stack

### (1) Run nginx on Kernel stack:

```shell
sudo ./nginx_nworker_kernel.sh <worker_num>
```

### (2) Run nginx on Light stack:
``` shell
sudo su
export LIGHT_SOURCE_PATH=<path_to_Light>
./nginx_nworker_Light.sh <nginx_worker_num>
```

The Light frontend module (FM) dynamic library `light_h6.so` is created from the source file `light_h.c`, which contains the network APIs of Light with POSIX semantics.
By leveraging `LD_PRELOAD` mechanism, the Light FM dynamic library has a higher priority than the default GNU C Library.
Thus when an application invokes network-related APIs, such as `socket(), listen(), bind()`, Light could take over and process them.

### Note: if you get the following error, you need to restart Light and nginx.

```shell
EAL: Detected 8 lcore(s)
EAL: Probing VFIO support...
EAL: WARNING: Address Space Layout Randomization (ASLR) is enabled in the kernel.
EAL:    This may cause issues with mapping memory into secondary processes
EAL: Could not mmap 2558525440 bytes in /dev/zero at [0x7ffef5a00000], got [0x7fc335475000] - please use '--base-virtaddr' option
EAL: It is recommended to disable ASLR in the kernel and retry running both primary and secondary processes
PANIC in rte_eal_init():
Cannot init memory
8: [/lib64/ld-linux-x86-64.so.2(+0xc6a) [0x7fc3cfd25c6a]]
7: [/lib64/ld-linux-x86-64.so.2(+0x107cb) [0x7fc3cfd357cb]]
6: [/lib64/ld-linux-x86-64.so.2(+0x106ba) [0x7fc3cfd356ba]]
5: [/home/tsinghua476/Light/light_h6.so(+0x19a6) [0x7fc3cfb219a6]]
4: [/usr/lib/light/liblightservice.so(light_app_init+0x55) [0x7fc3ce57dcd5]]
3: [/usr/lib/light/librte_eal.so.3.1(rte_eal_init+0x1145) [0x7fc3ce28e515]]
2: [/usr/lib/light/librte_eal.so.3.1(__rte_panic+0xcb) [0x7fc3ce28ce75]]
1: [/usr/lib/light/librte_eal.so.3.1(rte_dump_stack+0x2b) [0x7fc3ce29660b]]
Aborted (core dumped)
```