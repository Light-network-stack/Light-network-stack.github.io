## Light日志系统
###（日志系统仅用来调试，测性能时一定要disable）

### 一、配置说明
Light日志系统是基于syslog系统日志实现，现在的linux已改版为rsyslog，不过依然兼容syslog的配置。
 
#### 1. 添加rsyslog配置
在`/etc/rsyslog.conf`写入如下

```shell
local7.debug			/var/log/light/light.log
local7.info			/var/log/light/light_info.log
local7.warn			/var/log/light/light_warn.log
local7.err			/var/log/light/light_err.log
```
每个文件就会包括对应log级别及以上的log日志，具体更加灵活的配置请自行查阅网上讲解

#### 2. 创建并修改Light日志文件权限,执行如下命令

```shell
cd /var/log
mkdir light
sudo chown -R syslog:adm light //群组和用户必须修改才能正常工作
```
#### 3. 重启rsyslog
```
sudo service rsyslog restart
```

### 二、 日志开关控制

产生日志的分别是n个核运行的Light协议栈以及上层的应用程序，例如双核与nginx有三个进程会产生日志。
#### 1. 对于Light协议栈的日志在`startCorex.sh`脚本中修改 -l 参数，此参数代表可以输出的日志级别，如果配置成5或5以上则代表不输出日志，如果配置成2代表只输出warn和error级别的日志，数字含义请见`三、2`

#### 2. 对于应用程序需要修改`light_h.c`文件中argv参数中-l的配置，同上；然后重新运行`build_light_module`即可，产生新的so库。 

### 三、主要接口说明
#### 1. `light_log_init`设置log的输出方向

输入如下

```
#define LOG_TO_STDOUT 0
#define LOG_TO_SYSLOG 1
```

#### 2. `light_set_log_level`设置log输出级别
```
enum
{
	LIGHT_LOG_DEBUG,
	LIGHT_LOG_INFO,
	LIGHT_LOG_WARNING,
	LIGHT_LOG_ERR,
	LIGHT_LOG_CRIT,
	LIGHT_LOG_NONE
};
```
（分别对应0，1，2，3，4，5）

如果输入`LIGHT_LOG_WARNING`，则输出`LIGHT_LOG_WARNING`级别及以上级别的log

### 3. 使用说明

```
#ifdef DPDK_LINUX_INIT_LOG //通过宏定义开关
    light_log_init(LOG_TO_SYSLOG);
    light_set_log_level(LIGHT_LOG_WARNING);
#endif
```