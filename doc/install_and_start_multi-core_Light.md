# Compile and Run Light

### (1) Git clone the codes;
``` shell
git clone git@github.com:Light-network-stack/Light.git
```

### (2) Install gawk to avoid substituting `light_server_build.h`:
``` shell
sudo apt-get install gawk
```

### (3) Configure the IP address of Light in file `stack_and_service/service/dpdk_ip_stack_config.txt`;

### (4) Deactivate the NIC that DPDK uses later:
``` shell
sudo ifconfig <NIC_name> down
```

### (5) Configure DPDK:
``` shell
sudo $LIGHT_SOURCE_PATH/Light/dpdk-17.02/usertools/dpdk-setup.sh
```

Choose these steps in sequence:

`[13] x86_64-native-linuxapp-gcc`

`[16] Insert IGB UIO Module`

`[19] Setup hugepage mapping for non-NUMA systems`

The size of each hugepage memory is 2MB. We need 4096 pieces.

`[22] Bind Ethernet device to IGB UIO Module`

You can see all NICs. Please input the NIC name (e.g. ens3f0) or NIC address (e.g. 04:00.0) to bind it to the DPDK driver. 

Note: 
1) deactivate the NIC before binding; 
2) `ifconfig` will not show the NIC after it is bound to DPDK driver. 

`[33] Exit Script`

### (6) Compile DPDK and Light, and set up environment variables:
``` shell
sudo ./build_dev.sh
```

### (7) Run Light stack processes: 
``` shell
sudo su
export LIGHT_SOURCE_PATH=<path_to_Light>
```

#### 7.1) Method 1:

Main stack process (front end): 
``` shell
sudo ./startFirstCore.sh <total_stack_core_num>
```

Other stack processes (back end): 
``` shell
sudo ./startOtherCores.sh <total_stack_core_num>
```

#### 7.2) Method 2 (only for two cores):

Main stack process (front end): 
``` shell
sudo ./startFirstCore.sh 2
```

Other stack processes (front end): 
``` shell
sudo ./startSecondCores.sh 2
```

#### 7.3) Method 3:

Automatically run all Light processes on the backend. The process startup interval is 10s. 

``` shell
sudo ./start.sh <total_stack_core_num>
```

### (8) Kill all Light stack processes, Nginx processes and resource monitor process:

``` shell
sudo ./stop_all.sh
```
