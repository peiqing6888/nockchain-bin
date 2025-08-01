# 矿池状态监控功能使用说明

本文档介绍如何使用Nockchain矿池服务器中的状态监控功能。这些功能可以帮助矿场运营者实时监控矿池的运行状态、连接矿工数量、总哈希率、区块高度等关键指标。

## 功能概述

矿池状态监控系统包括以下几个组件：

1. **内置状态日志** - 矿池服务器会定期将运行状态记录到日志中
2. **HTTP API服务** - 提供实时查询矿池状态的接口
3. **监控脚本** - 用于在终端中显示矿池状态的命令行工具

## 配置参数

在`.env`文件中可以配置以下与状态监控相关的参数：

```
# HTTP API服务器监听地址
# 默认值为0.0.0.0:8080，可通过浏览器访问 http://<服务器IP>:8080/api/status 获取完整状态
HTTP_API_ADDRESS=0.0.0.0:8080

# 状态日志输出间隔（秒）
# 默认每300秒（5分钟）输出一次完整状态摘要到日志
STATUS_LOG_INTERVAL=300
```

## HTTP API接口

矿池服务器提供以下HTTP API接口：

1. **GET /api/status** - 获取完整的矿池状态信息
   - 响应包含：服务器信息、区块链信息、矿工信息、挖矿统计等
   - 示例URL: `http://localhost:8080/api/status`

2. **GET /api/basic** - 获取简化的矿池状态信息
   - 响应包含：连接矿工数、总哈希率、区块数量、当前区块高度
   - 示例URL: `http://localhost:8080/api/basic`

3. **GET /health** - 健康检查接口
   - 可用于监控系统检测服务是否在线
   - 示例URL: `http://localhost:8080/health`

## 使用监控脚本

我们提供了一个简单的监控脚本，用于在终端中实时显示矿池状态：

```bash
# 默认连接到本地矿池服务器
./scripts/monitor-pool.sh

# 指定矿池服务器地址
./scripts/monitor-pool.sh 192.168.1.100:8080
```

脚本要求系统安装了`curl`和`jq`命令。如果系统未安装，可以使用以下命令安装：

```bash
# Debian/Ubuntu
sudo apt install curl jq

# CentOS/RHEL
sudo yum install curl jq
```

## 日志中的状态信息

矿池服务器会定期在日志中输出状态摘要，格式如下：

```
[2023-07-23T13:00:00Z INFO] ============ 矿池状态摘要 ============
[2023-07-23T13:00:00Z INFO] 运行时间: 3600 秒
[2023-07-23T13:00:00Z INFO] 连接矿工: 10 (总线程数: 80)
[2023-07-23T13:00:00Z INFO] 当前难度: 000000fffff...
[2023-07-23T13:00:00Z INFO] 哈希率: 160000000 H/s
[2023-07-23T13:00:00Z INFO] 区块高度: 1234
[2023-07-23T13:00:00Z INFO] 最后区块时间: 2023-07-23T12:58:30Z
[2023-07-23T13:00:00Z INFO] 找到区块: 5 (已接受: 5)
[2023-07-23T13:00:00Z INFO] 当前工作ID: abcd1234
[2023-07-23T13:00:00Z INFO] 工作更新次数: 15
[2023-07-23T13:00:00Z INFO] ======================================
```

这些日志信息可以帮助您追踪矿池的历史运行状态，以及进行问题排查。

## 应用场景

1. **矿池监控** - 使用监控脚本或API实时监控矿池状态
2. **集成到监控系统** - 将HTTP API集成到Prometheus、Grafana等监控系统
3. **问题排查** - 通过历史日志分析矿池运行状态，排查问题

## 注意事项

1. HTTP API服务默认绑定到所有网络接口，如需限制访问，请修改`HTTP_API_ADDRESS`配置
2. 若在公网环境部署，建议配置防火墙规则限制对API端口的访问
3. API服务不包含身份验证机制，请通过网络层面控制访问权限 