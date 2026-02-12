[← Back to Topic](README.md) | [← Home](../../README.md)

# OpenUsage: AI Limits Tracker for AI Coding Subscriptions

## Index

- [Overview](#overview)
- [Workflow Fit](#workflow-fit)
- [Key Features](#key-features)
- [Supported Providers](#supported-providers)
- [Extensibility (Plugin Model)](#extensibility-plugin-model)
- [Installation and Platform Notes](#installation-and-platform-notes)
- [Sources](#sources)

---

## Overview

OpenUsage is a menu bar app that tracks how much of your AI coding subscriptions you’ve used, so you don’t hit provider limits by surprise. It aims to show usage “at a glance” without digging through dashboards, and it’s free/open source. [1] [2]

## Workflow Fit

The core value proposition is *situational awareness*: you can glance at current consumption, spot when you’re burning through quota unusually fast, and adjust your usage before getting blocked mid-session. [1] [2]

## Key Features

- **Menu bar UX:** Designed to live in the menu bar for one-glance visibility. [1] [2]
- **Background refresh:** Usage stats refresh automatically. [1] [2]
- **Account discovery:** The app claims it can automatically find linked accounts (e.g., Cursor, Claude Code) rather than requiring manual setup. [1]
- **Keyboard shortcut:** The panel can be toggled with a configurable global shortcut. [2]

## Supported Providers

OpenUsage tracks multiple AI coding tools via provider-specific plugins, including: Amp, Antigravity, Claude, Codex, Copilot, Cursor, Kimi Code, Windsurf, and Z.ai. [2]

The project also lists potential future additions (e.g., Gemini, Vercel AI Gateway). [2]

## Extensibility (Plugin Model)

OpenUsage treats each provider as a plugin, intending to make it easy for the community to add support for new vendors without redesigning the whole app. The repository points contributors to a Plugin API and notes that plugins are currently bundled but intended to become more flexible over time. [1] [2]

## Installation and Platform Notes

The recommended install path is downloading the latest GitHub release for macOS (Apple Silicon & Intel). The repository states the app auto-updates after installation, and calls out Windows/Linux support as a high-priority TODO needing testers. [2]

## Sources

1. [openusage.ai](https://www.openusage.ai/)
2. [github.com/robinebers/openusage](https://github.com/robinebers/openusage)

