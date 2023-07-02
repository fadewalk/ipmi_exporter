# 蓝鲸硬件监控插件

IPMI Exporter 是一种用于 Prometheus 监控系统的插件，它允许通过 IPMI（Intelligent Platform Management Interface）接口监控服务器的硬件指标和传感器数据。IPMI 是一种硬件管理接口，通常嵌入在服务器主板上，可以用于监控和远程管理服务器。

对于服务器硬件监控而言，IPMI Exporter 是非常有意义的。它可以提供服务器的关键硬件指标，如温度、风扇速度、电压、电流、功率等，帮助管理员了解服务器的健康状况和性能情况。通过实时监控这些指标，管理员可以及时发现和解决潜在的硬件问题，防止服务器硬件故障和性能下降。

以下是 IPMI Exporter 的一些主要优点：

- 实时监控：IPMI Exporter 提供了实时的硬件指标数据，可以在 Prometheus 监控系统中进行展示和分析。这样，管理员可以随时了解服务器的硬件状态，及时采取行动。

- 预警和报警：IPMI Exporter 可以配置警报规则，当硬件指标超过或低于预设的阈值时，可以触发警报通知管理员。这有助于管理员及时响应问题，避免潜在的硬件故障。

- 故障排除：当服务器出现问题时，IPMI Exporter 提供的硬件指标数据可以帮助管理员进行故障排除。通过分析硬件指标的变化，可以定位和诊断问题的根源。

总而言之，IPMI Exporter 对服务器的硬件监控非常有意义。它提供了实时的硬件指标数据，并支持警报和报警功能，有助于管理员及时发现和解决服务器硬件问题，保证服务器的正常运行和性能。

蓝鲸在主机基础监控CPU/Mem/IO/Disk已经内置采集,IPMI 硬件监控将会丰富蓝鲸的监控体系,让运维人员更加全面掌握企业服务器状态和企业的能效以及成本估算。

## 使用说明

### 插件功能

注意: 本插件监控的为物理服务器, 对于云主机、VMware 虚拟机并不适用。

ipmi_exporter 采用 IPMI 协议，底层是利用的物理服务器的带外管理BMC模块，监控服务器硬件的温度、电压、风扇、电源功率等。方便运维人员及时掌握服务器的硬件状态。

**使用本插件需要确保下发的机器安装了freeipmi 模块**

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

### 版本说明

为了方便进行版本管理和用户下载使用
本插件将按照  year.month.day 的格式对版本进行命名
如需使用的小伙伴记得下载最新的版本包

每次更新插件后,都会将在当前目录生成一个 
bk_ipmi_exporter_yymmdd.tgz 文件
大家可以下载这个包导入蓝鲸的监控平台插件
然后进行下发调试（调试前一定要确保安装了freeipmi 模块见前文）
