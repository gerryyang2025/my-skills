# My AI Skills

Personal AI Skills tool collection for Claude Code and other AI agents.

## Introduction

This repository contains a collection of AI skills (agents) that extend the capabilities of Claude Code and similar AI assistants. These skills provide specialized knowledge and tools for various tasks.

The skills are divided into two categories:
- **ClawHub**: Installed from ClawHub marketplace
- **Custom**: User-defined/customized skills

---

## ClawHub Skills

Skills installed from the [ClawHub](https://clawhub.club/) marketplace.

| Skill | Description |
|-------|-------------|
| [summarize](./skills/summarize/) | CLI tool to summarize URLs, files, PDFs, images, audio, and YouTube videos |
| [agent-browser](./skills/agent-browser/) | Rust-based headless browser automation for AI agents |
| [find-skills](./skills/find-skills/) | Discover and install agent skills from the open ecosystem |
| [obsidian](./skills/obsidian/) | Work with Obsidian vaults via obsidian-cli |
| [notion](./skills/notion/) | Notion API integration for pages, databases, and blocks |
| [weather](./skills/weather/) | Get current weather and forecasts (no API key required) |
| [tavily-search](./skills/tavily-search/) | AI-optimized web search via Tavily API |
| [github](./skills/github/) | Interact with GitHub using the `gh` CLI |
| [self-improving](./skills/self-improving/) | Self-reflection + self-criticism + self-learning agent |

---

## Custom Skills

User-defined skills tailored for personal use.

| Skill | Description |
|-------|-------------|
| [multi-search-engine](./skills/multi-search-engine/) | Multi search engine integration with 17 engines (8 CN + 9 Global) |
| [minimax-image-expert](./skills/minimax-image-expert/) | MiniMax image generation expert (image-01 model) |
| [minimax-music-expert](./skills/minimax-music-expert/) | MiniMax music creation expert (Music 2.5 model) |
| [us-stock-analyst](./skills/us-stock-analyst/) | US stock momentum analyst with daily reports |
| [http-fileserver](./skills/http-fileserver/) | HTTP file server management |
| [security](./skills/security/) | Security best practices for skills and configuration |

---

## Skills by Category

### Search & Research
- multi-search-engine (Custom) - 17 search engines integration
- tavily-search (ClawHub) - AI-optimized search
- summarize (ClawHub) - Content summarization

### Browser & Automation
- agent-browser (ClawHub) - Browser automation
- http-fileserver (Custom) - File serving

### Productivity Tools
- github (ClawHub) - GitHub CLI integration
- obsidian (ClawHub) - Obsidian notes
- notion (ClawHub) - Notion integration
- weather (ClawHub) - Weather information
- find-skills (ClawHub) - Skill discovery

### AI Content Generation
- minimax-image-expert (Custom) - Image generation
- minimax-music-expert (Custom) - Music generation

### Financial & Analysis
- us-stock-analyst (Custom) - US stock analysis

### Agent Enhancement
- self-improving (ClawHub) - Self-learning agent
- security (Custom) - Security guidelines

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/my-skills.git

# Navigate to the directory
cd my-skills
```

## Directory Structure

```
my-skills/
├── skills/
│   ├── multi-search-engine/      # Custom
│   ├── summarize/                # ClawHub
│   ├── agent-browser/            # ClawHub
│   ├── find-skills/              # ClawHub
│   ├── obsidian/                # ClawHub
│   ├── notion/                   # ClawHub
│   ├── weather/                  # ClawHub
│   ├── tavily-search/           # ClawHub
│   ├── github/                   # ClawHub
│   ├── self-improving/          # ClawHub
│   ├── minimax-image-expert/     # Custom
│   ├── minimax-music-expert/     # Custom
│   ├── us-stock-analyst/        # Custom
│   ├── http-fileserver/          # Custom
│   └── security/                 # Custom
├── README.md
├── LICENSE
├── CONTRIBUTING.md
└── CHANGELOG.md
```

## Usage

Each skill directory contains its own `SKILL.md` with detailed documentation. Refer to the specific skill's documentation for usage instructions.

## Contributing

Issues and Pull Requests are welcome! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

## License

MIT License
