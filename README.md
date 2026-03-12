# My AI Skills

Personal AI Skills collection for Cursor and other AI agents. Each skill is a `SKILL.md` with YAML frontmatter (`name`, `description`) that teaches the agent when and how to perform specialized tasks.

## Introduction

This repository contains **Agent Skills**: markdown-based instructions that extend AI assistants with domain knowledge and workflows. The agent uses each skill’s `description` to decide when to apply it, and the body of `SKILL.md` for step-by-step guidance.

Skills are grouped into:
- **Skillhub**: From the Skillhub marketplace (see table below)
- **Custom**: User-defined skills for personal or project use

---

## Skillhub Skills

Skills from the [Skillhub](https://skillhub.tencent.com/) marketplace.

| Skill | Description |
|-------|-------------|
| [summarize](./summarize/) | Summarize URLs, files, PDFs, images, audio, and YouTube via CLI |
| [agent-browser](./agent-browser/) | Rust-based headless browser automation for AI agents |
| [find-skills](./find-skills/) | Discover and install agent skills from the open ecosystem |
| [obsidian](./obsidian/) | Work with Obsidian vaults via obsidian-cli |
| [notion](./notion/) | Notion API for pages, databases, and blocks |
| [weather](./weather/) | Current weather and forecasts (no API key required) |
| [tavily-search](./tavily-search/) | AI-optimized web search via Tavily API |
| [github](./github/) | Interact with GitHub using the `gh` CLI |
| [tencent-docs](./tencent-docs/) | 腾讯文档：创建/查询/编辑智能文档、表格、幻灯片、思维导图等 |
| [tencentcloud-lighthouse-skill](./tencentcloud-lighthouse-skill/) | 腾讯云轻量应用服务器：mcporter/MCP、实例、监控、防火墙、快照、TAT |

---

## Custom Skills

User-defined skills for personal workflows.

| Skill | Description |
|-------|-------------|
| [multi-search-engine](./multi-search-engine/) | 17 search engines (8 CN + 9 global), advanced operators, WolframAlpha |
| [minimax-image-expert](./minimax-image-expert/) | MiniMax image generation (image-01, 文生图/图生图) |
| [minimax-music-expert](./minimax-music-expert/) | MiniMax music creation (Music 2.5, 歌词与音乐生成) |
| [minimax-video-expert](./minimax-video-expert/) | MiniMax 视频生成（文生视频、图生视频、首尾帧、主体参考、模板） |
| [us-stock-analyst](./us-stock-analyst/) | US stock momentum analyst with daily reports |
| [hk-stock-analyst](./hk-stock-analyst/) | 港股动量分析师，每日报告与量化策略 |
| [news-observer](./news-observer/) | 今日要闻深度解读生成器（财经/政治/科技/GitHub 热门） |
| [obsidian-publisher](./obsidian-publisher/) | 将 Obsidian 笔记发布为静态网站 |
| [http-fileserver](./http-fileserver/) | HTTP 文件服务器管理（端口与目录配置） |
| [security](./security/) | 安全规范：Skills 与配置最佳实践，防止敏感信息泄露 |

---

## Skills by Category

### Search & Research
- multi-search-engine (Custom) — 17 搜索引擎
- tavily-search (Skillhub) — AI 优化搜索
- summarize (Skillhub) — 内容摘要

### Browser & Automation
- agent-browser (Skillhub) — 浏览器自动化
- http-fileserver (Custom) — 文件服务

### Productivity & Docs
- github (Skillhub) — GitHub CLI
- obsidian (Skillhub) — Obsidian 笔记
- obsidian-publisher (Custom) — 笔记发布为静态站
- notion (Skillhub) — Notion
- tencent-docs (Custom) — 腾讯文档

### AI Content Generation
- minimax-image-expert (Custom) — 图像生成
- minimax-music-expert (Custom) — 音乐生成
- minimax-video-expert (Custom) — 视频生成

### Financial & Analysis
- us-stock-analyst (Custom) — 美股动量
- hk-stock-analyst (Custom) — 港股动量

### News & Trends
- news-observer (Custom) — 今日要闻解读

### Infrastructure & Cloud
- tencentcloud-lighthouse-skill (Custom) — 腾讯云轻量服务器

### Agent & Security
- find-skills (Skillhub) — 技能发现与安装
- self-improving (Skillhub) — 自反思与自学习
- security (Custom) — 安全规范
- weather (Skillhub) — 天气

---

## Skill Format (Cursor Agent Skills)

Each skill lives in a directory with a `SKILL.md` file:

- **YAML frontmatter**: `name` (≤64 chars, lowercase/hyphens) and `description` (what the skill does and when the agent should use it).
- **Body**: Instructions, examples, and optional links to `reference.md` or `examples.md`.

The agent uses `description` for discovery and the rest of `SKILL.md` for execution. Keep `SKILL.md` under ~500 lines; put long reference material in separate files.

---

## Quick Start

```bash
git clone https://github.com/yourusername/my-skills.git
cd my-skills
```

Use skills as a **project skill** by symlinking or copying the desired skill directory into your project’s `.cursor/skills/`, or as **personal skills** in `~/.cursor/skills/`. Cursor will load `SKILL.md` from those locations.

---

## Directory Structure

```
my-skills/
├── summarize/                    # Skillhub
├── agent-browser/                # Skillhub
├── find-skills/                  # Skillhub
├── obsidian/                     # Skillhub
├── notion/                       # Skillhub
├── weather/                      # Skillhub
├── tavily-search/                # Skillhub
├── github/                       # Skillhub
├── self-improving/               # Skillhub
├── multi-search-engine/          # Custom
├── minimax-image-expert/         # Custom
├── minimax-music-expert/         # Custom
├── minimax-video-expert/         # Custom
├── us-stock-analyst/             # Custom
├── hk-stock-analyst/             # Custom
├── news-observer/                # Custom
├── obsidian-publisher/           # Custom
├── tencent-docs/                 # Custom
├── tencentcloud-lighthouse-skill/# Custom
├── http-fileserver/              # Custom
├── security/                     # Custom
├── README.md
├── LICENSE
├── CONTRIBUTING.md
└── CHANGELOG.md
```

---

## Usage

Each skill directory contains a `SKILL.md` with usage and behavior. Refer to the specific skill’s `SKILL.md` for details.

## Contributing

Issues and Pull Requests are welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

MIT License
