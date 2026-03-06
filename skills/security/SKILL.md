---
name: security
description: OpenClaw 安全规范 Skill。确保所有 Skills 和配置遵循安全最佳实践，防止敏感信息泄露。
metadata: {"clawdbot":{"emoji":"🔒"}}
---

# OpenClaw 安全规范

确保所有 Skills 和配置遵循安全最佳实践。

## 环境变量

所有敏感配置通过环境变量管理，加载方式：

```bash
# 加载环境变量（必须在使用 API 前执行）
source /root/.openclaw/workspace/scripts/load-env.sh

# 验证
echo "MINIMAX_API_KEY: $MINIMAX_API_KEY"
echo "SERVER_IP: $SERVER_IP"
```

## 核心规则

### 1. 禁止在 Skills 中记录明文 API Key

**绝对禁止**在以下位置直接记录 API Key：
- SKILL.md 文件
- TOOLS.md 文件
- 任何 .md 文件
- 任何可被外部访问的文件

**正确做法**：将 API Key 配置在独立配置文件中

### 2. API Key 存储规范

**配置文件位置**：
```
/root/.openclaw/workspace/.config/api-keys.json
```

**配置文件格式**：
```json
{
  "minimax": {
    "api_key": "sk-xxx..."
  },
  "tavily": {
    "search_api_key": "tvly-xxx..."
  }
}
```

**文件权限**：
```bash
chmod 600 /root/.openclaw/workspace/.config/api-keys.json
```

### 3. Skill 中的 API Key 引用

在 Skill 文档中，使用变量引用而非明文：

**错误示例**：
```bash
-H "Authorization: Bearer sk-cp-xxx..."
```

**正确示例**：
```bash
-H "Authorization: Bearer ${MINIMAX_API_KEY}"
```

或者使用说明：
```
API Key 配置在：/root/.openclaw/workspace/.config/api-keys.json
```

### 4. HTTP 服务安全

- 确保敏感配置文件不在 HTTP 服务可访问目录
- 禁止目录遍历攻击
- 使用 bind mount 时设置只读

### 5. 定期检查

创建新 Skill 或修改现有 Skill 时，检查：
- [ ] 无明文 API Key
- [ ] 无敏感路径泄露
- [ ] 配置文件权限正确

---

## 快速检查命令

```bash
# 检查是否有明文 API Key
grep -r "sk-cp-\|tvly-\|d6g2tmpr\|f7201134" ~/.openclaw/workspace/skills/ --include="*.md"

# 检查配置文件权限
ls -la ~/.openclaw/workspace/.config/
```

---

## 违反规则的风险

1. **API Key 泄露** - 被恶意用户利用
2. **经济损失** - API 配额被消耗
3. **服务中断** - Key 被撤销或限流


### 6. 禁止在 Skills 中记录服务器 IP 地址

**绝对禁止**在以下位置直接记录服务器 IP：
- SKILL.md 文件
- TOOLS.md 文件
- 任何公开可访问的文件

**正确做法**：将服务器配置保存在独立配置文件中

**服务器配置文件位置**：
```
/root/.openclaw/workspace/.config/server.json
```

**配置文件格式**：
```json
{
  "http": {
    "ip": "{SERVER_IP}",
    "port": 8080
  }
}
```

**在 Skill 中引用**：
```
HTTP 服务 IP 和端口配置在：/root/.openclaw/workspace/.config/server.json
```

### 7. HTTP 服务文件编码

**问题**：浏览器访问 `.md`、`.py`、`.json` 等文件时可能出现乱码

**解决**：HTTP 服务配置统一的 UTF-8 编码

**server.py 配置示例**：
```python
extensions_map = {
    '.md': 'text/plain; charset=utf-8',
    '.py': 'text/plain; charset=utf-8',
    '.json': 'application/json; charset=utf-8',
    '.txt': 'text/plain; charset=utf-8',
    '.js': 'text/plain; charset=utf-8',
    '.html': 'text/html; charset=utf-8',
    '.css': 'text/css; charset=utf-8',
}
```

### 8. 生成的代码存储规范

**存储目录**：`/root/.openclaw/workspace/codes`

**分目录结构**：
```
/root/.openclaw/workspace/codes/
├── README.md              # 目录说明
├── http-server/           # HTTP 服务代码
│   ├── README.md
│   └── server.py
└── scripts/              # 工具脚本
    └── README.md
```

**安全要求**：
- 该目录**不通过浏览器对外访问**
- 只用于服务器内部代码存储
- 如需对外提供访问，使用 `/root/.openclaw/workspace/data` 目录

**目录说明**：
| 目录 | 用途 | HTTP 访问 |
|------|------|----------|
| `/root/.openclaw/workspace/codes` | 服务器代码、脚本 | ❌ 禁止 |
| `/root/.openclaw/workspace/data` | 用户生成文件 | ✅ 允许 |
| `/root/.openclaw/workspace/skills` | Skills 配置 | ✅ 只读 |

**每个子目录需创建 README.md**：简单介绍功能、用途和用法。
