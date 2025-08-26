# SNMP 网络监控系统

一个基于Go语言的SNMP网络监控系统，用于监控交换机MAC地址、接口和终端IP的对应关系。

## 功能特性

- 通过SNMP（UDP协议，端口161）从接入交换机获取MAC-接口对应关系
- 通过SNMP（UDP协议，端口161）从网关交换机获取IP-接口对应关系
- 将数据存储到MySQL数据库
- 提供Web界面显示网络拓扑关系
- 支持配置文件管理交换机IP列表

## 目录结构

```
.
├── cmd/                    # 主程序入口
├── internal/
│   ├── config/            # 配置管理
│   ├── database/          # 数据库连接和操作
│   ├── models/            # 数据模型
│   ├── snmp/              # SNMP客户端
│   ├── collector/         # 数据收集服务
│   └── web/               # Web服务器
├── web/                   # 前端文件
├── configs/               # 配置文件
├── scripts/               # 脚本文件
└── go.mod
```

## 使用方法

1. 配置数据库连接
2. 设置交换机IP列表
3. 运行程序：`go run cmd/main.go`
4. 访问 http://localhost:8080 查看监控界面