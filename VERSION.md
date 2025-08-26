# SNMP网络监控系统 版本信息

**产品名称**: SNMP网络监控系统  
**版本号**: v1.1  
**构建日期**: 2025-08-26  
**文件名**: snmp-network-monitor-v1.1.zip  

## 📦 发布包内容

### 核心文件
- `snmp-network-monitor.exe` (7.0MB) - 主程序可执行文件
- `start.bat` - 启动脚本
- `README.md` - 使用说明
- `INSTALL.md` - 安装配置指南
- `CHANGELOG.md` - 版本变更记录

### 目录结构
```
snmp-network-monitor/
├── snmp-network-monitor.exe    # 主程序
├── start.bat                   # 启动脚本
├── README.md                   # 使用说明
├── INSTALL.md                  # 安装指南
├── CHANGELOG.md                # 版本变更记录
├── web/                        # Web界面文件
│   ├── *.html                 # 页面文件
│   └── static/                # 静态资源
├── configs/                    # 配置文件目录
│   ├── access_switches.txt    # 接入交换机列表
│   └── gateway_switches.txt   # 网关交换机列表
└── scripts/                    # 脚本文件
    └── start.bat              # 详细启动脚本
```

## 🚀 快速开始

1. **解压文件**: 解压到目标目录
2. **配置数据库**: 首次运行会进入数据库配置模式
3. **配置交换机**: 编辑configs目录下的IP列表文件
4. **启动系统**: 双击 `start.bat` 或运行 `snmp-network-monitor.exe`
5. **访问界面**: 浏览器打开 http://localhost:8080

## 🔧 系统要求

- **操作系统**: Windows 7/8/10/11 (64位)
- **数据库**: MySQL 5.7+ 或 MariaDB 10.2+
- **内存**: 最少256MB可用内存
- **网络**: 支持SNMP协议的网络环境

## 📋 功能特性

- ✅ 自动数据库配置和初始化
- ✅ 数据库配置后自动重启（新增）
- ✅ 增强的状态信息显示（新增）
- ✅ SNMP数据采集（MAC地址表、ARP表）
- ✅ 网络拓扑可视化
- ✅ 多节点监控支持
- ✅ 实时数据更新
- ✅ Web管理界面
- ✅ 一键部署和启动

## 🐛 Bug修复 (v1.1)

- 修复系统状态显示交换机数量为0的问题
- 修复数据库配置后需要手动重启的问题
- 改进配置过程的用户体验

## 🔄 技术规格

- **开发语言**: Go 1.21+
- **Web界面**: HTML5 + CSS3 + JavaScript
- **数据库**: MySQL/MariaDB
- **协议支持**: SNMP v2c
- **架构**: 单体应用，内置Web服务器
- **部署**: 单可执行文件 + 静态资源

## 📞 技术支持

如需技术支持，请提供：
- 系统环境信息
- 完整的错误日志
- 网络和数据库配置信息

---
**构建信息**: Go build -ldflags="-s -w"  
**文件大小**: 约3MB（压缩包）  
**部署时间**: < 5分钟