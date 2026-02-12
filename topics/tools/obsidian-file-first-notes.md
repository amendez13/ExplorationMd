[← Back to Topic](README.md) | [← Home](../../README.md)

# Obsidian: File-First Notes, Migration, and CLI Search

## Index

- [Why “File Over App”](#why-file-over-app)
- [Migration Notes: Apple Notes → Obsidian](#migration-notes-apple-notes--obsidian)
- [Sync and Publishing Options](#sync-and-publishing-options)
- [Search and LLM Workflows](#search-and-llm-workflows)
- [Sources](#sources)

---

## Why “File Over App”

The core pitch is “file over app”: your notes live as plain-text Markdown files on your filesystem, which reduces vendor lock-in and makes it easier to reuse your content across tools and workflows [1].

This also enables direct use with LLMs (e.g., feeding hundreds of personal notes into an assistant) because the notes are already in a simple, machine-readable format [1].

## Migration Notes: Apple Notes → Obsidian

Obsidian provides an importer plugin to migrate from Apple Notes. Two practical details mentioned:

- If you have locked/encrypted Apple Notes, unlock them before importing [1].
- Review the import log; a small number of notes may require manual cleanup, but the plugin covers most of the work [1].

## Sync and Publishing Options

For a multi-device setup, iCloud can be used to sync the Markdown vault without paying for Obsidian Sync, and in that configuration Obsidian itself is not storing the user’s notes on an Obsidian-managed cloud [1].

The author also notes a correction: they meant they were considering supporting the project via Obsidian Publish (not Sync) [1]. Obsidian Publish pricing is referenced as starting at $25 USD [1].

Separately, the thread praises Obsidian’s business approach (staying small and funding without venture capital), crediting the vision of Obsidian’s CEO (“kepano”) [1].

## Search and LLM Workflows

The notes can be searched and summarized by an AI coding agent (the author mentions using Claude Code to review a directory of notes or search within them) [1].

They also claim Obsidian has introduced a CLI for more efficient searches (initially on Catalyst, with broader availability planned), which could make it easier to connect AI agents to a notes vault for faster retrieval and downstream synthesis [1].

## Sources

1. [x.com thread (Obsidian notes workflow)](https://x.com/i/status/2021534318506512672)

