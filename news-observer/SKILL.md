---
name: news-observer
description: 今日要闻深度解读生成器 - 扮演资深新闻编辑，实时抓取新闻并生成深度解读文章
metadata: {"clawdbot":{"emoji":"📰"}}
---

# 新闻观察 - 今日要闻深度解读

## 技能说明

扮演一位资深新闻编辑，生成【今日要闻深度解读】文章。

## 数据来源

### 1. 新闻搜索 (multi-search-engine)
使用 web_fetch 抓取多个新闻源：
- 财经：`https://news.china.com/zh_cn/finance/`
- 国内：`https://news.china.com/zh_cn/domestic/`
- 国际：`https://news.china.com/zh_cn/world/`
- 科技：`https://news.china.com/zh_cn/tech/`

### 2. GitHub Trending
使用 web_fetch 抓取 GitHub Trending：
- 今日：`https://github.com/trending`
- Python：`https://github.com/trending/python`
- JavaScript：`https://github.com/trending/javascript`

## 筛选标准

| 类别 | 数量 | 领域 |
|------|------|------|
| TOP 财经 | 10条 | 银行、证券、外贸、消费、能源 |
| TOP 政治 | 10条 | 国际关系、地缘政治、外交 |
| TOP 科技 | 10条 | 航天、AI、科研突破 |
| TOP GitHub | 10条 | 热门开源项目 |
| 今日热议 | 1个 | 社交媒体争议话题 |

## 输出结构

### 文章目录
```markdown
## 📋 目录

- [☁️ 天气](#天气)
- [📈 TOP 10 财经要闻](#top-10-财经要闻)
- [🌍 TOP 10 政治要闻](#top-10-政治要闻)
- [🚀 TOP 10 科技要闻](#top-10-科技要闻)
- [💻 TOP 10 GitHub 热门](#top-10-github-热门)
- [🔥 今日热议话题](#今日热议话题)
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
### 💻 [仓库名] - ⭐ stars

**描述：** 仓库简介

**语言：** 编程语言
```

## 生成脚本

```bash
/root/.openclaw/workspace/scripts/daily-news.sh
```

## 输出要求

- MD 文件：`today-news-YYYY-MM-DD.md`
- HTML 文件：`today-news-YYYY-MM-DD.html`
- 目录：`/root/.openclaw/workspace/data/obsidian/`
- 访问：`http://${SERVER_IP}:${SERVER_PORT}/obsidian/`（使用环境变量，需要先执行 `source /root/.openclaw/workspace/scripts/load-env.sh`）

## ⚠️ 重要注意事项

### HTML 渲染问题修复
生成 HTML 时需要注意：
1. **目录跳转问题**：marked 库默认不处理 Markdown 的 `{#id}` anchor 语法
2. **解决方案**：需要用 Node.js 后处理 HTML，手动添加 id 属性到 h2 标题
3. **代码示例**：
```javascript
// 添加 ID 到 h2 标题并清理 anchor 语法
html = html.replace(/<h2>([☁️📈🌍🚀💻🔥]\s*)([^<]+)\s*\{#([^}]+)\}<\/h2>/g,
  '<h2 id="$3">$1$2</h2>');
// 清理残留
html = html.replace(/\s*\{#[^}]+\}/g, '');
```

### GitHub 链接
- 仓库名需要添加可点击链接：`[仓库名](https://github.com/用户名/仓库名)`

## 定时任务

- 每天 7:35 自动执行
