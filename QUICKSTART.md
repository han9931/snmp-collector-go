# SNMP 网络监控系统 - 快速启动指南

## 系统要求

1. **Go环境**: Go 1.19 或更高版本
2. **MySQL数据库**: MySQL 5.7 或更高版本  
3. **网络设备**: 支持SNMP v2c协议的交换机设备

## 快速启动步骤

### 1. 安装和配置MySQL

确保MySQL服务已安装并运行。创建数据库用户（如果需要）：

```sql
-- 登录MySQL
mysql -u root -p

-- 创建用户（可选，也可以使用root用户）
CREATE USER 'netmonitor'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON network_monitor.* TO 'netmonitor'@'localhost';
FLUSH PRIVILEGES;
```

### 2. 修改数据库配置

编辑 `internal/config/config.go` 文件，更新数据库连接信息：

```go
Database: DatabaseConfig{
    Host:     "localhost",
    Port:     3306,
    Username: "root",          // 修改为您的用户名
    Password: "your_password", // 修改为您的密码
    DBName:   "network_monitor",
},
```

### 3. 配置交换机IP列表

#### 编辑接入交换机列表
编辑文件：`configs/access_switches.txt`
```
# 接入交换机IP列表（每行一个IP）
192.168.1.10
192.168.1.11
192.168.1.12
```

#### 编辑网关交换机列表  
编辑文件：`configs/gateway_switches.txt`
```
# 网关交换机IP列表（每行一个IP）
192.168.1.1
192.168.1.2
```

### 4. 验证网络连接

确保运行程序的主机能够通过SNMP访问所有配置的交换机。

**注意：SNMP使用UDP协议，端口161**

```bash
# 测试SNMP连接（在命令行中）
snmpwalk -v2c -c public 192.168.1.10 1.3.6.1.2.1.1.1.0

# 或者使用我们提供的测试工具
go run cmd/test_snmp.go 192.168.1.10
```

**重要提示**：
- SNMP使用UDP协议，不是TCP
- 使用`telnet`或`Test-NetConnection`测试TCP端口会失败，这是正常的
- UDP端口测试需要专门的工具或实际的SNMP请求

### 5. 启动程序

#### 方式一：使用启动脚本（Windows）
```cmd
scripts\start.bat
```

#### 方式二：手动编译运行
```cmd
go build -o bin\snmp-monitor.exe cmd\main.go
bin\snmp-monitor.exe
```

#### 方式三：直接运行
```cmd
go run cmd/main.go
```

### 6. 访问Web界面

程序启动成功后，打开浏览器访问：
```
http://localhost:8080
```

## 程序功能说明

### 数据收集
- 程序启动时立即执行一次数据收集
- 每5分钟自动收集一次数据
- 支持手动触发数据收集

### Web界面功能
1. **网络拓扑页面**: 显示MAC地址、接口、IP的完整对应关系
2. **MAC-接口页面**: 显示从接入交换机收集的数据
3. **IP-接口页面**: 显示从网关交换机收集的数据
4. **搜索功能**: 支持按关键词搜索
5. **系统状态**: 查看收集器状态和配置信息

### 数据库结构
程序会自动创建以下表：
- `mac_interface`: MAC-接口对应关系
- `ip_interface`: IP-接口对应关系  
- `network_topology`: 综合网络拓扑关系

## 故障排除

### 常见错误

1. **数据库连接失败**
   ```
   数据库初始化失败: 创建数据库失败: Error 1045
   ```
   解决方案：检查MySQL服务状态，验证用户名密码

2. **SNMP获取数据失败**
   ```
   从交换机 X.X.X.X 收集数据失败
   ```
   解决方案：
   - 检查网络连通性（ping命令）
   - 验证SNMP community设置
   - 确认设备SNMP服务已启用
   - **注意**：TCP端口161测试失败是正常的，SNMP使用UDP协议

3. **端口占用**
   ```
   Web服务器启动失败: listen tcp :8080: bind: address already in use
   ```
   解决方案：修改端口配置或关闭占用8080端口的程序

### 调试技巧

1. 查看详细日志输出来诊断问题
2. 使用 `ping IP` 测试网络连通性
3. **重要**：不要使用`telnet IP 161`测试SNMP，因为SNMP使用UDP协议，不是TCP
4. 使用我们提供的测试工具：`go run cmd/test_snmp.go <IP>`
5. 检查防火墙设置是否阻止UDP 161端口通信

## 生产环境部署建议

1. 使用专用数据库用户，避免使用root账户
2. 定期备份数据库数据
3. 考虑配置数据库连接池和超时设置
4. 部署在具有足够网络权限的服务器上
5. 考虑使用反向代理（如Nginx）提供HTTPS访问

## 自定义配置

如需要修改收集间隔、Web端口等配置，请编辑 `internal/config/config.go` 文件中的相应设置。