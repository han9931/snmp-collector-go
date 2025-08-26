# 配置说明

## 数据库配置

在使用前，请确保MySQL数据库已安装并运行。程序会自动创建数据库和表结构。

默认数据库配置：
- 主机: localhost
- 端口: 3306
- 用户名: root
- 密码: password
- 数据库名: network_monitor

如需修改数据库配置，请编辑 `internal/config/config.go` 文件中的 `LoadConfig()` 函数。

## 交换机IP配置

### 接入交换机IP列表
编辑文件：`configs/access_switches.txt`

示例：
```
# 接入交换机IP列表
192.168.1.10
192.168.1.11
192.168.1.12
```

### 网关交换机IP列表
编辑文件：`configs/gateway_switches.txt`

示例：
```
# 网关交换机IP列表
192.168.1.1
192.168.1.2
```

## SNMP配置

默认SNMP配置：
- Community: public
- Version: 2c
- Timeout: 5秒
- Retries: 3次

如需修改SNMP配置，请编辑 `internal/config/config.go` 文件。

## Web服务配置

默认Web服务配置：
- 监听地址: 0.0.0.0:8080

启动后可通过 http://localhost:8080 访问Web界面。

## 运行要求

1. Go 1.19 或更高版本
2. MySQL 5.7 或更高版本
3. 网络设备支持SNMP v2c协议
4. 确保程序运行主机能够访问所有配置的交换机IP

## 功能说明

### 数据收集
- 程序启动后会立即执行一次数据收集
- 之后每5分钟自动收集一次数据
- 可通过Web界面手动触发数据收集

### 数据存储
- MAC-接口对应关系存储在 `mac_interface` 表
- IP-接口对应关系存储在 `ip_interface` 表
- 综合拓扑关系存储在 `network_topology` 表

### Web界面
- 网络拓扑：显示完整的MAC、接口、IP对应关系
- MAC-接口：显示从接入交换机收集的MAC地址表
- IP-接口：显示从网关交换机收集的ARP表
- 支持搜索、分页等功能

## 故障排除

### 常见问题

1. **数据库连接失败**
   - 检查MySQL服务是否启动
   - 验证数据库配置信息
   - 确认用户权限

2. **SNMP获取数据失败**
   - 检查网络连通性
   - 验证SNMP配置（Community、版本）
   - 确认设备SNMP服务已启用

3. **Web页面无法访问**
   - 检查端口是否被占用
   - 验证防火墙设置
   - 确认程序是否正常启动

### 日志查看

程序运行时会输出详细的日志信息，包括：
- 数据收集过程
- 错误信息
- 系统状态

通过日志可以诊断大部分问题。