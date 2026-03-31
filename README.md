# WeChat Article to Obsidian

A Claude Code skill that saves WeChat public account articles (微信公众号文章) as clean Markdown notes in your Obsidian vault.

No browser needed. No plugins. No login. Just give it a link.

## Features

- **Zero dependencies** — only needs `curl` and `Node.js` (both pre-installed on most systems)
- **Clean Markdown output** — proper headings, bold/italic, code blocks, blockquotes, lists, links
- **Images preserved** — all article images kept as WeChat CDN links (renders in Obsidian)
- **Smart cleanup** — auto-removes WeChat decoration text (THUMB/STOPPING), merges PART headings, strips promotional tails (关注/点赞/在看)
- **YAML frontmatter** — title, author, publish date, source URL
- **Batch support** — save multiple articles at once
- **Natural language paths** — tell Claude where to save: "存到 reading/tech 目录"
- **One-time config** — set your vault name and default path once, then it just works

## How it works

WeChat MP articles are server-side rendered — the full article HTML is returned by a simple `curl` request. No JavaScript execution, no headless browser, no CDP needed. A Node.js script parses the HTML into clean Markdown and saves it to your Obsidian vault.

## Installation

Copy the `wechat-article-to-obsidian` folder to your Claude Code skills directory:

```bash
cp -r wechat-article-to-obsidian ~/.claude/skills/
```

## Usage

Just give Claude a WeChat article link:

```
帮我把这篇文章存到 Obsidian
https://mp.weixin.qq.com/s/xxxxx
```

On first use, Claude will ask you two questions:
1. Your Obsidian vault name
2. Default save path (e.g., `notes/wechat`)

After that, it's fully automatic.

### More examples

```
# Save to a specific folder
把这篇微信文章导入到 Obsidian 的 reading/ai 目录
https://mp.weixin.qq.com/s/xxxxx

# Batch save
帮我把下面几篇文章都存到 Obsidian
https://mp.weixin.qq.com/s/aaa
https://mp.weixin.qq.com/s/bbb
https://mp.weixin.qq.com/s/ccc
```

## Output example

```markdown
---
title: "Article Title Here"
author: "公众号名称"
publish_date: "2026-03-31 19:45:08"
saved_date: "2026-03-31"
source: "wechat"
url: "https://mp.weixin.qq.com/s/xxxxx"
---

# Article Title Here

Article content with **bold**, *italic*, `code`, and images preserved...
```

## File structure

```
wechat-article-to-obsidian/
├── SKILL.md        # Skill instructions for Claude
├── config.json     # Your vault config (auto-filled on first use)
├── README.md       # This file
└── scripts/
    ├── fetch.sh    # curl wrapper with browser-like headers
    └── parse.mjs   # HTML → Markdown parser with cleanup
```

## How articles are saved to Obsidian

The skill uses the [obsidian-cli](https://github.com/Yakitrak/obsidian-cli) to write notes into your vault:

```bash
obsidian create path="notes/wechat/Article Title.md" content="..." vault=MyVault
```

If `obsidian-cli` is not installed, it falls back to writing directly to your vault's disk path.

## Requirements

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [Obsidian](https://obsidian.md)
- [obsidian-cli](https://github.com/Yakitrak/obsidian-cli) (recommended — falls back to direct file write if not available)
- Node.js >= 18
- curl

## Limitations

- Some special article types (mini-programs, video-only posts) are not supported
- WeChat may rate-limit heavy usage — wait 30 seconds and retry
- Images use WeChat CDN links (`mmbiz.qpic.cn`) which may not render outside Obsidian/Typora

## License

MIT
