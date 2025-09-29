# anygo 多协议代理管理平台

> 继承 doubi 一键脚本风格，清爽干净无冗余

一个简洁高效的Xray Anytls服务端部署和Gost管理脚本，基于经典 gost.sh 脚本改进，最新主流协议支持，实现服务端搭建、中转机、落地机的一体化线路优化控制。

## ✨ 核心特性

- **🎨 doubi 风格** - 继承经典一键脚本的简洁设计理念
- **🚀 极速部署** - 仅依赖 Docker 官方镜像，不执行 apt update 安装一堆依赖
- **🔧 一体化方案** - 覆盖服务端、中转机、落地机的完整线路优化
- **📦 清爽干净** - 高速拉取官方镜像，AnyTLS 拉取官方GitHub 预编译文件 + systemd 管理
- **🎯 多协议集成** - Xray Reality、AnyTLS、GOST、Shadowsocks 四大主流协议
- **🛠️ 灵活转发** - 支持直接 TCP 端口转发和 TLS 加密隧道

## 🎯 设计理念

**不像其他脚本那样臃肿：**
- ✅ 直接使用 Docker 官方高速镜像。拉去镜像-搭建成功-只需数十秒
- ✅ 所有服务统一管理，配置清晰可见

- ❌ 不需要 `apt update` 安装一大堆系统依赖
- ❌ 不需要担心VPS被安装各种琐碎服务

## 🚀 快速开始

### 系统要求

- Linux 系统 (CentOS/Debian/Ubuntu)
- ROOT 权限
- Docker (脚本自动安装)

### 一键安装

```bash
wget -O https://ghfast.top/https://raw.githubusercontent.com/vince-ankunding/anygo/refs/heads/main/anygo.sh
chmod +x anygo.sh
sudo ./anygo.sh
```

## 📋 支持的协议

### 1. Xray Reality+Vision

最新高性能抗封锁协议

**特点:**
- 自动生成 UUID 和 ShortID
- 多种预设 SNI 域名可选

### 2. AnyTLS 加密隧道

轻量级 TLS 伪装方案

**技术细节:**
- 使用 GitHub 官方预编译超小体积二进制
- 低资源占用，高效稳定

### 3. GOST 多协议代理

功能全面的转发平台，脚本核心

**支持模式:**
- TCP/UDP 不加密直连转发
- TLS 加密隧道发送到落地机
- 落地机 TLS 隧道解密
- 灵活的多级路由配置

### 4. Shadowsocks

经典稳定的加密代理，不出境首选，多用于隧道内服务端


## 🎯 主要功能

### 服务部署

```
[1] 搭建服务端
    ├── Xray Reality+Vision - 高性能抗封锁
    ├── AnyTLS 加密隧道 - 轻量级 TLS 伪装
    ├── GOST 多协议代理 - 功能全面转发
    └── Shadowsocks 服务端 - 经典加密代理
```

- 自动分配随机端口 (10000-20000)
- 自动生成认证信息 (UUID/密码)
- 即时显示分享链接和二维码

### 服务管理

```
[2] 服务配置与管理
    ├── 实时状态监控
    ├── 连接信息查看
    ├── 端口/密码修改
    ├── 加密方式切换
    ├── 日志实时查看
    └── 服务重启/停止
```

### 转发配置（GOST 核心功能）

**三种转发模式:**

1. **TCP/UDP 不加密中转**
   - 适用场景：中转机到落地机，协议自带加密
   - 配置简单，性能最优

2. **TLS 加密隧道发送**
   - 适用场景：中转机加密发送到落地机
   - 需要落地机执行解密

3. **TLS 隧道解密**
   - 适用场景：落地机接收并解密 TLS 隧道
   - 配合中转机加密使用

### 一体化线路方案

```
用户端 → 中转机(anygo) → 落地机(anygo) → 目标
         ├── 部署代理服务端
         ├── GOST TLS 加密转发
         └── 统一管理界面
```

## 🔧 技术架构

### Docker 容器管理

- **Xray**: `ghcr.io/xtls/xray-core:latest`
- **GOST**: `ginuerzh/gost:2.12`
- 使用 `--network host` 模式
- 自动重启策略 `--restart unless-stopped`

### AnyTLS 服务管理

- 从 GitHub Releases 获取预编译二进制
- systemd 单元文件管理
- 支持多架构 (amd64/arm64/armv7)

### 配置文件结构

```
/root/net-tools-anygo/
├── xray-config.json      # Xray 服务配置
├── anytls-config.json    # AnyTLS 服务配置
├── gost-config.json      # GOST 服务配置（自动生成）
└── rawconf               # 转发规则原始配置
```

## 📖 使用场景

### 场景 1: 单服务端部署

快速搭建 Xray Reality 服务供个人使用

```bash
主菜单 → 1.搭建服务端 → 1.Xray Reality+Vision → 回车随机端口
```

### 场景 2: 中转机优化线路

使用 GOST 中转优化高延迟线路

```bash
# 中转机操作
主菜单 → 7.新增转发规则 → 2.TLS加密隧道 → 配置端口和落地机IP

# 落地机操作
主菜单 → 7.新增转发规则 → 3.隧道解密 → 配置端口和目标服务
```

### 场景 3: 多协议混合部署

在同一台服务器部署多种协议

```bash
1. 部署 Xray Reality (端口 10001)
2. 部署 Shadowsocks (端口 10002)
3. 部署 AnyTLS (端口 10003)
4. 统一在服务管理界面查看
```

## 🛡️ 安全建议

- 使用随机端口避免被扫描
- 定期更换密码和 UUID
- 启用强加密算法 (aes-256-gcm / chacha20)
- 监控服务日志发现异常
- 及时更新 Docker 镜像

## 📊 性能优势

**对比传统脚本：**

| 对比项 | 传统脚本 | anygo |
|--------|---------|-------|
| 依赖安装 | apt update + 多个包 | 仅需 Docker |
| 部署时间 | 5-10 分钟 | 1-2 分钟 |
| 系统污染 | 安装大量依赖 | 仅容器隔离 |
| 更新维护 | 手动重新编译 | 一键拉取镜像 |
| 配置管理 | 分散多处 | 统一目录 |

## 📝 常见问题

**Q: 为什么选择 Docker 而不是编译安装？**  
A: Docker 官方镜像经过优化测试，部署快速且隔离性好，避免污染系统环境

**Q: GOST 转发和直接部署服务端有什么区别？**  
A: GOST 转发适合线路优化，可以实现中转机到落地机的灵活配置

**Q: AnyTLS 为什么使用 systemd 而不是 Docker？**  
A: AnyTLS 是轻量级工具，systemd 管理更简洁，且便于查看日志和控制

**Q: 如何实现多级中转？**  
A: 使用 GOST 的 TLS 隧道功能，中转机加密发送，落地机解密，可串联多级

**Q: 脚本会保留配置吗？**  
A: 所有配置保存在 `/root/net-tools-anygo/`，重新运行脚本会保留现有配置

## 🎨 界面预览

```
<img width="455" height="470" alt="image" src="https://github.com/user-attachments/assets/5723e2dd-e0d0-4dd9-9581-cb6443e1a0f7" />

```

## 🤝 致谢

- 基于经典 gost.sh 脚本改进
- 继承 doubi 脚本的设计风格
- 感谢开源社区的各项目支持

## 📄 许可证

本项目采用开源许可证发布

## ⚠️ 免责声明

本工具仅供使用者学习和研究网络技术使用。

---

**版本:** v1.1.0  
**风格:** doubi 经典简洁风  
**更新:** 持续维护中
