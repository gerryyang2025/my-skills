---
name: news-observer
description: 今日要闻深度解读生成器 - 扮演资深新闻编辑，实时抓取新闻并生成深度解读文章
metadata: {"clawdbot":{"emoji":"📰"}}
---

# 新闻观察 - 今日要闻深度解读

## 技能说明

扮演一位资深新闻编辑，生成【今日要闻深度解读】文章。

## 数据来源

### 1. 新闻搜索 (web_fetch)
使用 web_fetch 抓取多个新闻源：
- 财经：`https://news.china.com/zh_cn/finance/`
- 国内：`https://news.china.com/zh_cn/domestic/`
- 国际：`https://news.china.com/zh_cn/world/`
- 科技：`https://news.china.com/zh_cn/tech/`

### 2. GitHub Trending
使用 web_fetch 抓取 GitHub Trending：
- 今日：`https://github.com/trending`
- 本周：`https://github.com/trending?since=weekly`

### 3. Dev.to AI Research

**使用 API 获取 AI 文章**：
```
https://dev.to/api/articles?tag=ai&per_page=10
```

API 返回 JSON 数据，字段说明：
- `url` - 文章完整链接
- `title` - 文章标题
- `description` - 文章摘要
- `user.name` - 作者名
- `readable_publish_date` - 发布日期
- `tags` - 标签

**示例**：
```json
{
  "title": "AI Writes Code. You Own Quality",
  "url": "https://dev.to/helderberto/ai-writes-code-you-own-quality-19g0",
  "description": "The more I use AI tools...",
  "user": { "name": "Helder" },
  "tags": "ai,programming"
}
```

### 4. 天气
```bash
curl -s "wttr.in/Shenzhen?format=%t"
```

## 筛选标准

| 类别 | 数量 | 领域 |
|------|------|------|
| TOP 财经 | 10条 | 银行、证券、外贸、消费、能源 |
| TOP 国内 | 10条 | 社会、民生、政策 |
| TOP 科技 | 10条 | 航天、AI、科研突破 |
| TOP GitHub | 10条 | 热门开源项目 |
| TOP AI Research | 10条 | Dev.to 最新 AI 研究文章 |
| 今日热议 | 1个 | 社交媒体争议话题 |

## 输出结构

### 文章目录
```markdown
## 📋 目录

- [☁️ 天气](#weather)
- [📈 TOP 10 财经要闻](#finance)
- [🌍 TOP 10 国内要闻](#domestic)
- [🚀 TOP 10 科技要闻](#tech)
- [💻 TOP 10 GitHub 热门](#github)
- [📚 TOP 10 AI Research](#ai-research)
- [🔥 今日热议话题](#hot-topic)
```

### 每条新闻三段式
```markdown
### N️⃣ [新闻标题]

**事件：** 50字以内简述核心事实

**背景/原因：**
- 原因1

**影响/展望：**
- 👤 受益者/受损者
```

### GitHub 仓库格式
```markdown
### 💻 [仓库名](链接) - ⭐ stars

**描述：** 仓库简介

**语言：** 编程语言
```

### Dev.to AI 文章格式
```markdown
### 📄 [文章标题](原文链接)

**简介：** 文章摘要

**作者：** 作者名

**标签：** #AI #ML
```

## 输出要求

- MD 文件：`today-news-YYYY-MM-DD.md`
- HTML 文件：运行 `node generate.js` 自动生成
- 目录：`/root/.openclaw/workspace/data/obsidian/`
- 访问：`http://${SERVER_IP}:${SERVER_PORT}/obsidian/`（使用环境变量，需要先执行 `source /root/.openclaw/workspace/scripts/load-env.sh`）

## 生成步骤

```bash
# 1. 创建 Markdown 文件
cd /root/.openclaw/workspace/data/obsidian
vim today-news-2026-03-25.md

# 2. 运行 ob publisher 生成 HTML
cd /root/.openclaw/workspace/codes/obsidian-publisher
node generate.js
```

## ⚠️ 重要注意事项

### Dev.to API 使用
- ❌ 不要使用 `/t/ai` 页面（需要登录）
- ✅ 必须使用 API：`https://dev.to/api/articles?tag=ai&per_page=10`
- API 无需认证，完全公开

### HTML 渲染问题修复
生成 HTML 时需要注意：
1. **目录跳转问题**：marked 库默认不处理 Markdown 的 `{#id}` anchor 语法
2. **解决方案**：需要用 Node.js 后处理 HTML，手动添加 id 属性到 h2 标题
3. **代码示例**：
```javascript
// 添加 ID 到 h2 标题并清理 anchor 语法
html = html.replace(/<h2>([☁️📈🌍🚀💻🔥📄]\s*)([^<]+)\s*\{#([^}]+)\}<\/h2>/g,
  '<h2 id="$3">$1$2</h2>');
// 清理残留
html = html.replace(/\s*\{#[^}]+\}/g, '');
```

### GitHub 链接
- 仓库名需要添加可点击链接：`[仓库名](https://github.com/用户名/仓库名)`

## 定时任务

- 每日要闻生成：每天 7:35
