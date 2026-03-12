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
- **访问地址**: `http://${SERVER_IP}:${SERVER_PORT}/`（使用环境变量，需要先执行 `source /root/.openclaw/workspace/scripts/load-env.sh`）

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

## 常用命令

| 操作 | 命令 |
|------|------|
| 启动 | `systemctl start openclaw-http.service` |
| 停止 | `systemctl stop openclaw-http.service` |
| 状态 | `systemctl status openclaw-http.service` |
| 重启 | `systemctl restart openclaw-http.service` |
| 日志 | `journalctl -u openclaw-http.service -n 50` |

---

## 加固配置（推荐）

生产环境建议使用加固后的 systemd 服务配置：

```bash
cat > /etc/systemd/system/openclaw-http.service << 'EOF'
[Unit]
Description=OpenClaw HTTP File Server
After=network.target
StartLimitIntervalSec=300
StartLimitBurst=5

[Service]
Type=simple
User=root
WorkingDirectory=/root/.openclaw/workspace/data
# 启动前清理占用端口的进程
ExecStartPre=-/bin/bash -c 'lsof -ti:8080 | xargs -r kill -9 || true'
ExecStart=/usr/bin/python3 -m http.server 8080 --bind 0.0.0.0
# 失败时重启，防止无限重启
Restart=on-failure
RestartSec=5
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl restart openclaw-http.service
```

### 加固说明

| 配置 | 作用 |
|------|------|
| `ExecStartPre` | 启动前自动清理占用端口的旧进程 |
| `StartLimitBurst=5` | 5分钟内最多重启5次，防止无限重启 |
| `Restart=on-failure` | 仅在失败时重启，避免正常stop触发重启 |

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

## 访问服务

```
http://${SERVER_IP}:${SERVER_PORT}/
```

---

## 故障排除

### 端口被占用

```bash
# 查看占用端口的进程
lsof -i :8080

# 强制杀掉
lsof -ti:8080 | xargs kill -9
```

### 服务启动失败

```bash
# 查看详细日志
journalctl -u openclaw-http.service -n 50

# 手动启动测试
cd /root/.openclaw/workspace/data
python3 -m http.server 8080
```

### 外网无法访问

- 检查云服务器安全组是否开放 8080 端口
- 检查防火墙：`firewall-cmd --list-ports`

---

## 云服务器端口配置

**重要**：需要在云控制台安全组开放 8080 端口！

---

## 目录结构

```
/root/.openclaw/workspace/data/        # HTTP 根目录
├── obsidian/                         # Obsidian 笔记
│   ├── index.html
│   ├── obsidian-guide.html
│   ├── oc-guide.html
│   └── szyffx.html
├── h5-snake-game/                    # H5 游戏
└── ...其他文件...
```
