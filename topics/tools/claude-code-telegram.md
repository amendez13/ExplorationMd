[← Back to Topic](README.md) | [← Home](../../README.md)

# Claude Code Telegram Bot

A Telegram bot that provides remote access to Claude Code through natural language chat. Enables mobile-friendly interaction with codebases from any device without terminal access.

## Index

- [Overview](#overview)
- [Operating Modes](#operating-modes)
- [Core Features](#core-features)
- [Security Model](#security-model)
- [Notifications and Webhooks](#notifications-and-webhooks)
- [Setup](#setup)
- [Use Cases](#use-cases)
- [Sources](#sources)

---

## Overview

Claude Code Telegram bridges conversational AI coding assistance with mobile accessibility. Instead of requiring SSH or terminal access to run Claude Code, developers can interact with their codebases through Telegram's chat interface.

The bot maintains session persistence per user and project directory, enabling continuous workflows across devices. Users ask questions or request changes in plain language; the bot routes those to Claude Code and returns results.

## Operating Modes

**Agentic Mode (Default):**
Conversational interface requiring only natural language. Minimal commands:
- `/start` - Begin session
- `/new` - Fresh context
- `/status` - Check session state

**Classic Mode:**
Terminal-like interface with structured navigation for advanced users:
- `/cd`, `/ls`, `/pwd` - Directory navigation
- `/projects` - Switch projects
- `/export` - Export sessions (Markdown, HTML, JSON)
- `/actions` - View available operations
- `/git` - Safe repository operations

## Core Features

**Codebase Interaction:**
- Natural language queries for code analysis, editing, and explanation
- File and archive uploads with automatic extraction
- Image analysis for screenshots and diagrams
- Git integration with safe repository operations

**Session Management:**
- Automatic persistence per user/project
- Session export in multiple formats
- Usage and cost tracking per user
- Complete audit logging

**Technical Architecture:**
- Full Claude Code integration via Python SDK (CLI fallback available)
- SQLite persistence with automatic migrations
- Token bucket rate limiting
- Path traversal prevention for directory isolation

## Security Model

Multi-layer authentication and isolation:

- **User Whitelist:** Only approved Telegram user IDs can access the bot
- **Optional Token Auth:** Additional token-based authentication layer
- **Directory Sandboxing:** Operations restricted to approved directories; path traversal blocked
- **Request Limits:** Per-user spending and request caps
- **Input Validation:** Protection against injection attacks
- **Audit Trail:** Complete logging of all operations

## Notifications and Webhooks

**FastAPI Webhook Server:**
- GitHub HMAC-SHA256 signature verification
- Bearer token authentication
- Per-chat rate limiting

**Scheduler:**
- Cron-based job scheduling with persistent storage
- Event bus for decoupled message routing
- Proactive notifications from scheduled tasks

## Setup

**Prerequisites:**
- Python 3.10+
- Poetry
- Claude Code CLI installed
- Telegram Bot Token (via @BotFather)

**Installation:**
```bash
git clone https://github.com/RichardAtCT/claude-code-telegram.git
cd claude-code-telegram
make dev
cp .env.example .env
```

**Minimal Configuration (.env):**
- `TELEGRAM_BOT_TOKEN` - From @BotFather
- `TELEGRAM_BOT_USERNAME` - Bot identifier
- `APPROVED_DIRECTORY` - Base project directory
- `ALLOWED_USERS` - Comma-separated Telegram user IDs

**Running:**
- `make run` - Production mode
- `make run-debug` - With logging

## Use Cases

- **Mobile code review:** Analyze and explain code while away from workstation
- **CI/CD notifications:** Automated summaries from GitHub webhooks
- **Daily health checks:** Scheduled codebase analysis via cron
- **Quick diagnostics:** Bug investigation without SSH access
- **Team alerts:** Notifications on repository events

## Sources

1. [RichardAtCT/claude-code-telegram](https://github.com/RichardAtCT/claude-code-telegram)
