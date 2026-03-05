---
name: http-fileserver
description: HTTP 文件服务器管理技能。快速启动/配置常驻的 HTTP 服务用于文件下载，支持指定端口和目录。
metadata: {"clawdbot":{"emoji":"🌐"}}
---

# HTTP 文件服务器管理

快速启动和管理 HTTP 文件服务器，用于提供文件下载服务。

## 默认配置

- **端口**: 8080
- **目录**: /root/.openclaw/workspace/data
- **访问地址**: `http://{IP}:8080/`（IP 配置在 `/root/.openclaw/workspace/.config/server.json`）

## 快速使用

### 启动服务

```bash
# 启动默认服务
systemctl start openclaw-http.service
```

### 停止服务

```bash
# 停止服务
systemctl stop openclaw-http.service
```

---

## 手动配置步骤

### 步骤 1：创建 systemd 服务文件

```bash
cat > /tmp/openclaw-http.service << 'EOFSERVICE'
[Unit]
Description=OpenClaw HTTP File Server
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory={DIRECTORY}
ExecStart=/usr/bin/python3 -m http.server {PORT} --directory {DIRECTORY} --bind 0.0.0.0
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOFSERVICE
```

### 步骤 2：部署服务

```bash
cp /tmp/openclaw-http.service /etc/systemd/system/
systemctl daemon-reload
systemctl enable openclaw-http.service
systemctl start openclaw-http.service
```

---

## 常用命令

| 操作 | 命令 |
|------|------|
| 启动 | `systemctl start openclaw-http.service` |
| 停止 | `systemctl stop openclaw-http.service` |
| 状态 | `systemctl status openclaw-http.service` |
| 重启 | `systemctl restart openclaw-http.service` |

---

## 访问服务

```
http://{IP}:8080/
```
（IP 配置在 `/root/.openclaw/workspace/.config/server.json`）

---

## 云服务器端口配置

**重要**：需要在云控制台安全组开放 8080 端口！

