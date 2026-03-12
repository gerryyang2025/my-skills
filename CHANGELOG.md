# Changelog

All notable changes to this project will be documented in this file.


## [0.3.0] - 2026-03-12

### Changed
- **README**: Aligned with Cursor Agent Skills — SKILL.md format (YAML frontmatter `name`/`description`), agent discovery and usage
- **README**: Skill source renamed ClawHub → **Skillhub**
- **README**: Directory structure updated — skills live at repo root, not under `skills/`
- **README**: Added "Skill Format (Cursor Agent Skills)" and usage (project vs personal skills)
- **CHANGELOG**: Terminology and skill list aligned with README

### Removed
- **.skills_store_lock.json**: No longer used; Skillhub vs Custom is indicated by the README tables only


---

## [0.2.0] - 2026-03-06

### Added

#### Skillhub Skills (from Skillhub marketplace)
- **summarize**: CLI tool to summarize URLs, local files, PDFs, images, audio, and YouTube videos
- **agent-browser**: Rust-based headless browser automation CLI for AI agents (navigation, click, type, snapshot)
- **find-skills**: Skill discovery and installation from the open agent skills ecosystem
- **obsidian**: Work with Obsidian vaults using obsidian-cli
- **notion**: Notion API for pages, databases, and blocks
- **weather**: Current weather and forecasts (no API key required)
- **tavily-search**: AI-optimized web search via Tavily API
- **github**: Interact with GitHub using the `gh` CLI (issues, PRs, CI runs)
- **self-improving**: Self-reflection + self-criticism + self-learning + self-organizing memory agent

#### Custom Skills (User-defined)
- **multi-search-engine**: 17 search engines (8 CN + 9 global), advanced operators, time filters, WolframAlpha
- **minimax-image-expert**: MiniMax image generation (image-01, text-to-image / image-to-image)
- **minimax-music-expert**: MiniMax music creation (Music 2.5, lyrics and music generation)
- **us-stock-analyst**: US stock momentum analyst with daily reports (Marcus Goldman persona)
- **http-fileserver**: HTTP file server management (port and directory config)
- **security**: Security best practices for skills and configuration

## [0.1.0] - 2026-03-05

### Added
- Project initialization
- Added README.md
- Added LICENSE
- Added .gitignore
- Added CONTRIBUTING.md
