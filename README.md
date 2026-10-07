# LWBRUT — Autonomous Code Intelligence & PR Review Engine

[![License: MIT](https://img.shields.io/badge/License-MIT-purple.svg)](https://opensource.org/licenses/MIT)
[![MCP Protocol](https://img.shields.io/badge/MCP-Protocol%20Ready-06b6d4.svg)](https://modelcontextprotocol.io/)
[![Powered by](https://img.shields.io/badge/Intelligence-Claude%203.5%20Sonnet-a855f7.svg)](https://anthropic.com)
[![Tree-sitter](https://img.shields.io/badge/AST-Tree--sitter%20Deterministic-10b981.svg)](https://tree-sitter.github.io/)

LWBRUT is a developer intelligence platform combining deterministic **Tree-sitter Abstract Syntax Tree (AST)** validation with **Anthropic Claude 3.5 Sonnet** deep reasoning. It analyzes multi-file pull requests to eliminate concurrency deadlocks, race hazards, and security regressions before merging.

Official Website: [https://lwbrut.com](https://lwbrut.com)

---

## ⚡ Core Architecture

```
PR Event (GitHub / GitLab)
       │
       ▼
┌──────────────────────────────────────────────┐
│ 1. Git Diff Ingestion & Symbol Tree Parsing │  (Tree-sitter AST)
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ 2. Cross-File Symbol Graph Resolution        │  (Extract callers & type contracts)
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ 3. Deep Reasoning via Claude 3.5 Sonnet      │  (Zero-day data retention tier)
└──────────────────────┬───────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────┐
│ 4. Deterministic AST Verification Gate       │  (Suppresses phantom types/APIs)
└──────────────────────┬───────────────────────┘
                       │
                       ▼
Automated GitHub Pull Request Inline Annotations
```

---

## 🔌 Model Context Protocol (MCP) Integration

LWBRUT natively implements the Anthropic **Model Context Protocol (MCP)**, allowing teams to run AST-verified reviews directly from **Claude Desktop** or local CLI environments.

### Claude Desktop Configuration

Add the following to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "lwbrut": {
      "command": "npx",
      "args": ["-y", "@lwbrut/mcp-server"]
    }
  }
}
```

---

## 🛡️ Data Isolation & Zero Retention

- **Ephemeral Compute:** Code is processed purely in volatile RAM during PR review runs.
- **Zero Training:** Powered by Anthropic's commercial API tier with contractual guarantees that customer code is **never** used to train public or proprietary models.
- **AST Gate:** Synthesized suggestions that reference non-existent variables, unimported types, or phantom methods are discarded before publishing comments.

---

## 🚀 Quick Start (GitHub Actions)

Add LWBRUT to your repository via `.github/workflows/lwbrut.yml`:

```yaml
name: LWBRUT Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
      - name: Run LWBRUT Engine
        uses: lwbrut/action@v1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          rules_file: ".lwbrut/rules.md"
```

---

## 📄 License

Licensed under the [MIT License](LICENSE).
Copyright © 2026 LWBRUT Inc. Built on Anthropic Frontier Intelligence.
