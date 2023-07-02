# 蓝鲸监控插件

## 使用说明

### 插件功能

ipmi_exporter 采用 IPMI 协议，底层是利用的物理服务器的带外管理BMC模块，监控服务器硬件的温度、电压、风扇、电源功率等。方便运维人员及时掌握服务器的硬件状态。

使用本插件需要确保下发的机器安装了freeipmi 模块

```
yum install -y freeipmi
# apt install -y freeipmi
```


### 版本支持


操作系统支持: linux

本插件包默认Linux 服务器监控，更多其他服务器监控 (点击下载)[https://github.com/prometheus-community/ipmi_exporter]



**是否支持远程采集:**

是

本插件默认采集的是下发的宿主机硬件指标，如需远程采集，需要配置 ` --config.file=CONFIG.FILE` 可以在参数那里指定

### 参数说明

```
root@zjz:/apps# ./ipmi_exporter -h
usage: ipmi_exporter [<flags>]

Flags:
  -h, --help                     Show context-sensitive help (also try --help-long and --help-man).
      --config.file=CONFIG.FILE  Path to configuration file.
      --freeipmi.path=FREEIPMI.PATH
                                 Path to FreeIPMI executables (default: rely on $PATH).
      --web.listen-address=":9290"
                                 Address to listen on for web interface and telemetry.
      --web.config.file=""       [EXPERIMENTAL] Path to configuration file that can enable TLS or authentication.
      --log.level=info           Only log messages with the given severity or above. One of: [debug, info, warn, error]
      --log.format=logfmt        Output format of log messages. One of: [logfmt, json]
      --version                  Show application version.

```


### 配置参数


| **参数名**                       | **含义**                           | **是否必填** | **使用举例**                |
|-------------------------------|----------------------------------|----------|-------------------------|
| --config.file                 | 配置文件路径(如果需要配置采集参数或者远程采集需要指定配置文件) | 否        | /etc/ipmi-exporter.yaml |
| --freeipmi.path=FREEIPMI.PATH | freeipmi 模块地址。 yum/apt 必须安装该模块   | 否        |                         |
| --web.listen-address          | 监听端口                             | 是        | 127.0.0.1:19290         |
| --web.config.file=""          | [实验] 可以启用 TLS 或身份验证的配置文件的路径。     | 否        |                         |
| --log.level=info              | 日志级别                             | 否        | info                    |
| --log.format=logfmt           | 日志格式                             | 否        | json                    |



### 指标简介


| 指标ID                                      | 指标中文名    | 维度ID                                                            | 维度含义   | 单位     |
|-------------------------------------------|----------|-----------------------------------------------------------------|--------|--------|
| ipmi_chassis_power_state                  | 机箱电源状态   |                                                                 |        |        |
| ipmi_dcmi_power_consumption_current_watts | 功率消耗当前瓦数 |                                                                 |        |        |
| ipmi_scrape_duration_seconds              | 抓取持续时间   |                                                                 |        |        |
| ipmi_bmc_info                             | bmc信息    | system_firmware_version firmware_revision manufacturer_id       | 系统固件版本 | string |
| ipmi_sensor_state                         | 传感器状态    |                                                                 |        |        |
| ipmi_current_amperes                      | 电流A      |                                                                 |        |        |
| ipmi_current_state                        | 当前状态     |                                                                 |        |        |
| ipmi_fan_speed_rpm                        | 风扇转速rpm  |                                                                 |        |        |
| ipmi_fan_speed_state                      | 风扇速度状态   |                                                                 |        |        |
| ipmi_power_state                          | 电源状态     |                                                                 |        |        |
| ipmi_power_watts                          | 电源瓦      | 这个指标一般不是反应实际功率消耗(建议使用ipmi_dcmi_power_consumption_current_watts) |        |        |
| ipmi_temperature_celsius                  | 温度       |                                                                 |        |        |
| ipmi_temperature_state                    | 温度状态     |                                                                 |        |        |
| ipmi_voltage_state                        | 电压状态     |                                                                 |        |        |
| ipmi_voltage_volts                        | 电压V      |                                                                 |        |        |


### 版本日志

#### ipmi_exporter 

基于 ipmi_exporter v1.6.1 制作

### 版本

为了方便进行版本管理和用户下载使用
本插件将按照
year.month.day 的格式对版本进行命名
如需使用的小伙伴记得下载最新的版本包

