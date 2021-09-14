# Light Stack

<!-- ![rocket_Light_word](./rocket_Light_word.svg) -->
<img src="/image/rocket_Light_word.png" alt="Light" width="126"/>

Light is a compatible, high-performance and scalable user-level network stack.

<!-- Here is [the document of Light stack](https://light-network-stack.github.io/). -->

## Introduction

As the CPU core number and the Ethernet NIC speed keep increasing on physical machines, the kernel stack has become the bottleneck for applications demanding high throughput and low latency. 
Recently there is a growing tendency towards moving the network stack out of the kernel. However, most kernel-bypass network stacks abandon POSIX APIs that legacy applications depend on, and the complicated work of transplanting applications hinders the real-world deployment of kernel-bypass stacks.

Light is a novel user-level network stack, which not only gains highly scalable performance on multi-core server, but also achieves compatibility with exisiting applications. 
For high performance and scalability, Light employs lock-free shared queue based inter-process communication and full connection affinity to reduce overheads of lock, system call, cache miss, etc. 
For compatibility, Light realizes efficient blocking APIs in the user space, intercepts network APIs in a non-intrusive way, and adopts the FD space separation mechanism for appropriate API redirection. 

According to evaluation results, various kinds of legacy applications could run on Light without modifying source code. 
In comparison with the latest kernel stack, Nginx on Light achieves up to 2.86× throughput and 78.2% lower tail latency (99.9th percentile) with 14 CPU cores.

Light is open-source and released under GPL-2.0.


## Quick Start

### Compile and Run Light
See [Compile and Run Light](/doc/install_and_start_multi-core_Light.html).

### Run Nginx
See [Nginx guide](/doc/run_nginx_on_kernel_or_Light.html).

### Logs for Debugging
See [Light log manual](/doc/light_log_manual.html).


## Performance Results

### Test environment
```
NIC: Intel 82599ES 10GbE
CPU: Intel Xeon E5-2683 v4 @ 2.10GHz
Memory: 128G DDR3
```

### Throughput
Average requests per second (RPS) of Nginx on LIGHT and kernel stack under different core numbers. The webpage size is set as 64 bytes.

<img src="/image/results_rps_cores.JPG" alt="Light-Throughput" width="450"/>

In comparison with the latest kernel stack, Nginx on Light achieves up to 2.86× throughput with 14 CPU cores.

### Latency
HTTP response latency of Nginx on LIGHT and kernel stack under different core numbers. The webpage size is set as 64 bytes. 
The bottom border, internal line and top border of each box indicate the 25th, 50th and 75th percentile of the data. 
The whiskers represent the 0.1th and 99.9th percentile.

<img src="/image/results_latency_cores.JPG" alt="Light-Latency" width="450"/>

With 14 CPU cores, Nginx on Light has 78.2% lower tail latency (99.9th percentile) than the latest kernel stack.


## Contact
For any questions, please send an email to hotjunfeng[at]163[dot]com.
