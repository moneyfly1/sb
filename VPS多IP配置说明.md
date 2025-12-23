# VPS多IP配置说明

## 概述

本说明文档介绍如何在拥有多个IP地址的VPS上配置sing-box，使每个IP都能生成独立的节点。

## 前提条件

1. VPS拥有多个IP地址（示例：4个IP）
2. 所有IP地址都已正确配置在VPS上
3. 每个IP地址都能正常访问互联网

## VPS网络配置检查

### 1. 检查所有IP地址

```bash
# 方法1：使用ip命令
ip addr show

# 方法2：使用ifconfig命令
ifconfig

# 方法3：查看所有网络接口
ip -4 addr | grep inet
```

### 2. 验证IP地址可达性

```bash
# 测试每个IP是否可达
ping -c 3 66.253.7.112
ping -c 3 66.253.5.69
ping -c 3 66.253.5.113
ping -c 3 66.253.5.159
```

### 3. 检查IP路由配置

```bash
# 查看路由表
ip route show

# 检查特定IP的路由
ip route get 66.253.7.112
```

## 可能需要的VPS设置

### 1. 确保所有IP都已绑定到网络接口

如果某些IP未正确绑定，需要手动添加：

```bash
# 为eth0接口添加IP（示例）
ip addr add 66.253.7.112/24 dev eth0
ip addr add 66.253.5.69/24 dev eth0
ip addr add 66.253.5.113/24 dev eth0
ip addr add 66.253.5.159/24 dev eth0

# 或者使用ifconfig（某些系统）
ifconfig eth0:1 66.253.7.112 netmask 255.255.255.0 up
ifconfig eth0:2 66.253.5.69 netmask 255.255.255.0 up
ifconfig eth0:3 66.253.5.113 netmask 255.255.255.0 up
ifconfig eth0:4 66.253.5.159 netmask 255.255.255.0 up
```

### 2. 配置防火墙规则

确保防火墙允许所有IP的流量：

```bash
# Ubuntu/Debian系统
ufw allow from any to any port 20000:21000 proto tcp
ufw allow from any to any port 20000:21000 proto udp

# CentOS系统
firewall-cmd --permanent --add-port=20000-21000/tcp
firewall-cmd --permanent --add-port=20000-21000/udp
firewall-cmd --reload

# 或者直接关闭防火墙（不推荐生产环境）
systemctl stop firewalld
systemctl disable firewalld
```

### 3. 配置IP转发（如果需要）

```bash
# 启用IP转发
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p
```

### 4. 确保每个IP都有正确的路由

```bash
# 检查默认路由
ip route show default

# 如果需要为特定IP添加路由
ip route add default via <网关IP> dev eth0 src 66.253.7.112
```

## 脚本使用说明

### 1. 使用优化后的脚本

优化后的脚本会自动：
- 检测VPS上的所有IP地址
- 为每个IP分配不同的端口范围
- 为每个IP生成4个协议的节点配置
- 生成所有IP的节点分享链接

### 2. 端口分配规则

每个IP使用独立的端口范围，间隔1000：
- IP1: 20000-20003 (Vless, Vmess, Hysteria2, Tuic5)
- IP2: 21000-21003
- IP3: 22000-22003
- IP4: 23000-23003

### 3. 节点配置输出

脚本会为每个IP生成：
- Vless-reality节点
- Vmess-ws节点
- Hysteria2节点
- Tuic5节点

所有节点配置保存在：
- `/etc/s-box/ip_port_mapping.txt` - IP和端口映射
- `/etc/s-box/nodes_ip*.txt` - 每个IP的节点链接

## 故障排查

### 问题1：某些IP无法访问

**解决方案：**
1. 检查IP是否正确绑定到网络接口
2. 检查防火墙规则
3. 检查路由配置
4. 联系VPS提供商确认IP配置

### 问题2：端口被占用

**解决方案：**
脚本会自动检测并调整端口，如果仍有问题：
1. 检查端口占用：`ss -tunlp | grep <端口>`
2. 手动修改端口范围

### 问题3：节点无法连接

**解决方案：**
1. 检查sing-box服务状态：`systemctl status sing-box`
2. 查看日志：`journalctl -u sing-box -f`
3. 验证IP和端口配置是否正确
4. 检查客户端配置是否匹配

## 注意事项

1. **IP数量限制**：建议不超过10个IP，避免端口冲突
2. **端口范围**：确保端口范围不与系统服务冲突
3. **防火墙**：生产环境建议配置精确的防火墙规则
4. **性能**：多个IP和端口可能增加系统负载
5. **备份**：修改网络配置前请备份重要数据

## 联系支持

如遇到问题，请提供：
1. VPS系统信息（`uname -a`）
2. 网络配置信息（`ip addr show`）
3. 脚本运行日志
4. sing-box运行日志

