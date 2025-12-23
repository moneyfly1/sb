# Sing-box-yg 多IP支持增强版

基于 [yonggekkk/sing-box-yg](https://github.com/yonggekkk/sing-box-yg) 脚本增强开发，新增**多IP节点自动配置功能**。

> **致谢**：本项目基于甬哥（yonggekkk）的优秀脚本增强，感谢原作者的贡献！

## ✨ 新增功能

### 🚀 多IP节点自动配置

- **自动检测所有IP地址**：脚本会自动检测VPS上的所有IPv4地址
- **智能端口分配**：为每个IP自动分配独立的端口范围，避免冲突
- **批量节点生成**：每个IP自动生成4个协议节点（Vless-reality、Vmess-ws、Hysteria2、Tuic5）
- **一键配置**：无需手动配置，选择菜单选项即可完成所有IP的节点配置

### 支持的协议

* **Vless-reality-vision** - 最流行的Reality协议
* **Vmess-ws(tls)/Argo** - 支持TLS和Argo隧道
* **Hysteria-2** - 高性能UDP协议
* **Tuic-v5** - 新一代QUIC协议

## 📦 一键安装脚本

### 方式一：使用 wget（推荐）

```bash
bash <(wget -qO- https://raw.githubusercontent.com/moneyfly1/sb/main/sb.sh)
```

### 方式二：使用 curl

```bash
bash <(curl -Ls https://raw.githubusercontent.com/moneyfly1/sb/main/sb.sh)
```

## 🎯 使用方法

### 1. 安装Sing-box

运行脚本后，选择菜单选项 `1` 进行安装：

```bash
bash <(wget -qO- https://raw.githubusercontent.com/moneyfly1/sb/main/sb.sh)
# 然后选择 1
```

### 2. 配置多IP节点

安装完成后，在主菜单选择 **`17. 多IP节点配置（内置多IP）`**

脚本会自动：
- ✅ 检测VPS上的所有IP地址
- ✅ 为每个IP分配端口（默认从20000开始，每个IP间隔1000）
- ✅ 生成配置文件并重启服务
- ✅ 保存IP和端口映射到 `/etc/s-box/ip_port_mapping.txt`

### 3. 查看节点信息

配置完成后，选择菜单选项 `9` 查看所有节点的分享链接和二维码。

## 📊 端口分配规则

每个IP使用独立的端口范围（间隔1000）：

| IP序号 | Vless | Vmess | Hysteria2 | Tuic5 |
|--------|-------|-------|----------|-------|
| IP1    | 20000 | 20001 | 20002    | 20003 |
| IP2    | 21000 | 21001 | 21002    | 21003 |
| IP3    | 22000 | 22001 | 22002    | 22003 |
| IP4    | 23000 | 23001 | 23002    | 23003 |

## 📁 配置文件位置

- **主配置文件**：`/etc/s-box/sb.json`
- **IP端口映射**：`/etc/s-box/ip_port_mapping.txt`
- **所有IP列表**：`/etc/s-box/all_ips.txt`
- **节点链接文件**：`/etc/s-box/nodes_ip*.txt`

## 🔧 VPS网络配置要求

### 检查IP配置

```bash
# 查看所有IP地址
ip addr show | grep "inet "

# 或使用
ifconfig | grep "inet "
```

### 配置防火墙

```bash
# Ubuntu/Debian
ufw allow 20000:25000/tcp
ufw allow 20000:25000/udp

# CentOS
firewall-cmd --permanent --add-port=20000-25000/tcp
firewall-cmd --permanent --add-port=20000-25000/udp
firewall-cmd --reload
```

### 确保IP可达

```bash
# 测试每个IP
ping -c 3 <你的IP地址>
```

详细配置说明请查看：[VPS多IP配置说明.md](VPS多IP配置说明.md)

## 📖 完整功能列表

脚本主菜单包含以下功能：

1. 一键安装 Sing-box
2. 删除卸载 Sing-box
3. 变更配置（双证书TLS/UUID路径/Argo/IP优先/TG通知/Warp/订阅/CDN优选）
4. 更改主端口/添加多端口跳跃复用
5. 三通道域名分流
6. 关闭/重启 Sing-box
7. 更新 Sing-box-yg 脚本
8. 更新/切换/指定 Sing-box 内核版本
9. 刷新并查看节点（Clash-Meta/SFA+SFI+SFW三合一配置/多合一订阅链接/推送TG通知）
10. 查看 Sing-box 运行日志
11. 一键原版BBR+FQ加速
12. 管理 Acme 申请域名证书
13. 管理 Warp 查看Netflix/ChatGPT解锁情况
14. 添加 WARP-plus-Socks5 代理模式
15. 双栈VPS切换IPV4/IPV6配置输出
16. Sing-box-yg脚本使用说明书
17. **多IP节点配置（内置多IP）** ⭐ 新增功能
0. 退出脚本

## 🎬 使用示例

假设您的VPS有多个IP地址：

1. **运行脚本**：
   ```bash
   bash <(wget -qO- https://raw.githubusercontent.com/moneyfly1/sb/main/sb.sh)
   ```

2. **选择1安装Sing-box**（如果还未安装）

3. **选择17配置多IP节点**：
   - 脚本自动检测所有IP地址
   - 自动分配端口范围
   - 为每个IP生成4个协议节点

4. **选择9查看节点**：
   - 查看所有IP的节点分享链接
   - 扫描二维码添加到客户端

## ⚠️ 注意事项

1. **IP数量限制**：建议不超过10个IP，避免配置过于复杂
2. **端口范围**：确保20000-25000端口范围未被占用
3. **防火墙**：生产环境建议配置精确的防火墙规则
4. **备份配置**：修改前建议备份 `/etc/s-box/sb.json`

## 🔍 故障排查

### 问题1：检测不到所有IP

**解决方案**：
```bash
# 检查IP是否正确绑定
ip addr add <IP地址>/24 dev eth0

# 查看路由
ip route show
```

### 问题2：节点无法连接

**解决方案**：
```bash
# 检查服务状态
systemctl status sing-box

# 查看日志
journalctl -u sing-box -f

# 验证配置文件
cat /etc/s-box/sb.json | jq .
```

### 问题3：端口被占用

脚本会自动检测并调整端口。如果仍有问题：
```bash
# 查看端口占用
ss -tunlp | grep <端口号>
```

## 📚 相关文档

- [VPS多IP配置说明.md](VPS多IP配置说明.md) - 详细的VPS网络配置指南
- [快速使用指南.md](快速使用指南.md) - 快速上手指南
- [集成指南.md](集成指南.md) - 技术集成说明

## 🙏 致谢

- **原项目**：[yonggekkk/sing-box-yg](https://github.com/yonggekkk/sing-box-yg) - 感谢甬哥（yonggekkk）的优秀脚本
- **Sing-box项目**：[SagerNet/sing-box](https://github.com/SagerNet/sing-box)
- **本项目**：[moneyfly1/sb](https://github.com/moneyfly1/sb) - 多IP增强版

## 📄 声明

本项目基于 [yonggekkk/sing-box-yg](https://github.com/yonggekkk/sing-box-yg) 脚本增强开发，新增多IP节点自动配置功能。

- **原作者**：甬哥（yonggekkk）
- **增强开发**：moneyfly1
- **主要增强**：多IP检测、自动端口分配、批量节点生成

所有代码来源于Github社区与ChatGPT的整合。

---

**⭐ 如果这个项目对您有帮助，请给个Star支持一下！**
