# Obsidian Publisher - 笔记发布工具

将 Obsidian 笔记转换为静态网站，支持多种部署方式。

## 目录结构

```
obsidian-publisher/
├── SKILL.md           # 本文件
├── generate.js        # Markdown 转 HTML 生成器
├── template.html      # 页面模板
└── package.json       # Node.js 依赖
```

## 快速开始

### 1. 安装依赖

```bash
cd /root/.openclaw/workspace/quartz-output
npm install marked
```

### 2. 生成静态网站

```bash
cd /root/.openclaw/workspace/quartz-output
node generate.js
```

### 3. 部署到 HTTP 服务

```bash
# 复制到 HTTP 服务目录
cp -r /root/.openclaw/workspace/quartz-output /root/.openclaw/workspace/data/obsidian
```

### 4. 访问

- 访问：`http://${SERVER_IP}:${SERVER_PORT}/obsidian/`（使用环境变量，需要先执行 `source /root/.openclaw/workspace/scripts/load-env.sh`）
- 笔记列表: http://${SERVER_IP}:${SERVER_PORT}/obsidian/
- 笔记详情: http://${SERVER_IP}:${SERVER_PORT}/obsidian/xxx.html

## 配置说明

### 源笔记目录

默认从 `/root/.openclaw/workspace/data/obsidian` 读取 Markdown 文件。

如需修改，编辑 `generate.js` 中的 `VAULT_DIR` 变量：

```javascript
const VAULT_DIR = "/你的笔记目录路径";
```

### 输出目录

默认输出到 `/root/.openclaw/workspace/quartz-output`。

修改 `generate.js` 中的 `OUTPUT_DIR`：

```javascript
const OUTPUT_DIR = "/你的输出目录路径";
```

### 模板修改

编辑 `template.html` 自定义页面样式。

## 支持的功能

- ✅ Markdown 转 HTML
- ✅ YAML frontmatter 解析（title, tags, date）
- ✅ 代码高亮
- ✅ 标签显示
- ✅ 复选框样式
- ✅ 响应式布局（手机友好）
- ✅ Wiki 链接 [[note]]
- ✅ 英文文件名（避免中文编码问题）

## 📝 笔记写作规范（必读）

创建新笔记时，请务必遵循以下规范：

### 1. 文件命名

- 中文标题会自动转为英文 slug
- 标题中应包含明确关键词，如"港股每日动量报告"

### 2. Frontmatter 格式

```yaml
---
title: 港股每日动量报告 - 2026年3月11日
date: 2026-03-11
tags: [港股, 每日报告, 动量报告, 梁文涛]
---
```

**注意**：
- `tags` 使用数组格式 `[tag1, tag2, tag3]`
- 不要在 tags 中使用方括号包裹单个标签，如 `[#梁文涛]` ❌
- 正确：`tags: [港股, 梁文涛]` ✅

### 3. 正文更新时间

在文章正文开头自动添加更新时间（由 generate.js 自动生成）：

```html
<p class="update-time">更新时间：2026-03-11</p>
```

该字段取自 frontmatter 中的 `date` 字段。

### 4. 标题规范

- 文章内的一级标题 `#` 应包含完整标题，如 `# 📊 港股每日动量报告 - 2026年3月11日`
- 不要遗漏关键词（港股、美股等）

### 4. 发布流程

每次新增或修改笔记后，必须执行：

```bash
# 1. 生成静态网页
cd /root/.openclaw/workspace/quartz-output
node generate.js

# 2. 复制到 HTTP 服务目录
cp *.html /root/.openclaw/workspace/data/obsidian/
```

### 5. 常见错误避免

| 错误 | 正确做法 |
|------|---------|
| tags: [#港股] | tags: [港股] |
| 标题无关键词 | 标题包含"港股"、"美股"等 |
| 修改后未重新生成 | 每次修改后运行 generate.js |

### 6. 定时任务配合

如使用 cron 自动生成报告，确保：
- 任务包含完整的生成和发布流程
- 任务执行后用户可通过网站查看

## Obsidian 2026 新特性

根据 2026 年最新信息：

### AI 集成

- **Smart Connections**: AI 自动关联笔记，笔记间自动建立链接
- **AI Transcriber**: 集成 Whisper 语音转文字，转为 Markdown
- 可接入多种主流 AI API（ChatGPT、Claude 等）

### 新版插件

| 插件 | 功能 |
|------|------|
| Bases | 多维表格与逻辑计算 |
| Tasks | 仪表盘视图，一键管理全局高优任务 |
| Templater | 流处理器，自动填充日记模板 |
| Obsidian to Anki | 导出笔记到 Anki 记忆卡片 |

### 2026 工作流建议

> "最稳的路线就是保留刚需插件，让工具够用，稳定、不拖你后腿。"

推荐工作流：
- 保留刚需插件，避免功能堆砌
- 专注写作，减少阻力
- 利用本地优先特性，保护隐私

### 国内使用加速

由于 Obsidian 主题/插件资源在 GitHub，建议使用 **Steam++** (瓦特工具箱) 加速访问。

## 部署方式

### 方式一：HTTP 服务（当前使用）

笔记目录：`/root/.openclaw/workspace/data/obsidian`
访问地址：`http://${SERVER_IP}:${SERVER_PORT}/obsidian/`（使用环境变量，需要先执行 `source /root/.openclaw/workspace/scripts/load-env.sh`）

### 方式二：GitHub Pages

1. 将输出目录 `quartz-output` 初始化为 git 仓库
2. 推送到 GitHub
3. 启用 GitHub Pages

### 方式三：对象存储

1. 将 `quartz-output` 目录打包
2. 上传到阿里云 OSS / 腾讯云 COS
3. 配置自定义域名

### 方式四：iCloud/OneDrive 同步

1. 将 vault 文件夹放到云盘同步目录
2. 手机端安装云盘 App + Obsidian
3. 跨设备无缝同步

## 定时更新（可选）

如需自动同步，可设置 cron 任务：

```bash
# 每天凌晨 2 点自动生成并部署
0 2 * * * cd /root/.openclaw/workspace/quartz-output && node generate.js && cp -r /root/.openclaw/workspace/quartz-output /root/.openclaw/workspace/data/obsidian
```

## 故障排除

### 页面显示空白

检查生成是否成功：
```bash
ls -la /root/.openclaw/workspace/quartz-output/
```

### 中文文件名无法访问

确保使用英文文件名，或配置 URL 编码支持。本系统已配置自动转英文 slug。

### 样式丢失

确认 HTTP 服务支持 HTML 文件：
```bash
curl -s http://127.0.0.1:8080/obsidian/ | head -5
```

### 新笔记未显示

重新运行生成脚本：
```bash
cd /root/.openclaw/workspace/quartz-output && node generate.js
```

### 外网无法访问

- 检查云服务器安全组是否开放 8080 端口
- 检查防火墙：`firewall-cmd --list-ports`

## 相关链接

- Obsidian 官网: https://obsidian.md
- Obsidian 中文论坛: https://forum.obsidian.md
- marked (Markdown 解析): https://marked.js.org/
- Steam++ 加速器: https://steampp.net/
