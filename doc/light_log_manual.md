# Light Log Manual

Light logs are based on [RSyslog](https://www.rsyslog.com/) and used for debugging.
Light logs should be disabled when using Light for high performance.


## Setup

### (1) Create log directory
```shell
cd /var/log
mkdir light
sudo chown -R syslog:adm light # The group and user have to be changed
```

### (2) Modify RSyslog config file

Open `/etc/rsyslog.conf` and add the following content:
```shell
local7.debug			/var/log/light/light.log
local7.info			/var/log/light/light_info.log
local7.warn			/var/log/light/light_warn.log
local7.err			/var/log/light/light_err.log
```

Every file includes the logs at the same and higher severity levels.

### (3) Restart RSyslog
```shell
sudo service rsyslog restart
```


## Configuration

The Light stack and application can create logs, and we can configure the log severity level.

There are 6 levels (from 0 to 5), i.e. `LIGHT_LOG_DEBUG, LIGHT_LOG_INFO, LIGHT_LOG_WARNING, LIGHT_LOG_ERR, LIGHT_LOG_CRIT, LIGHT_LOG_NONE`.

### (1) Log level of Light

Change the `log_level` parameter in the shell scripts of Light, such as `startFirstCore.sh`. 
If the `log_level` is 2, the Light logs whose level are equal or higher than `LIGHT_LOG_WARNING` are enabled.
If the `log_level` is 5, all Light logs are disabled. 

### (2) Log level of applications

Change the `APP_LOG_LEVEL` value in `light_h.c`. 
If the `APP_LOG_LEVEL` is 2, the application logs whose level are equal or higher than `LIGHT_LOG_WARNING` are enabled.
If the `APP_LOG_LEVEL` is 5, all application logs are disabled. 

Then re-compile the Light FM:

```shell
sudo ./build_light_module.sh
```


## APIs

### (1) `light_log_init()`
Set the output location of logs, i.e. STDOUT and SYSLOG.

```c
void light_log_init(int loc)
```

#### Parameter:
loc: the output location of logs. The value could be LOG_TO_STDOUT or LOG_TO_SYSLOG.

### (2) `light_log()`
Add a log at a specific log level.

```c
inline void light_log(int level, const char* format, ...)
```

#### Parameter:
level: log level.

format, ...: log content.

#### Example:
Add an info-level log “LOG_CONTENT”.

```c
light_log(LIGHT_LOG_INFO, "LOG_CONTENT\n");
```
