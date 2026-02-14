[← Back to Topic](README.md) | [← Home](../../README.md)

# Summarize (summarize.sh): Offline-First Link, File, and Media Summaries

## Index

- [What It Is](#what-it-is)
- [Interfaces](#interfaces)
- [Offline-First and Caching](#offline-first-and-caching)
- [Media Pipeline (Transcripts and Fallbacks)](#media-pipeline-transcripts-and-fallbacks)
- [Slides Mode (YouTube)](#slides-mode-youtube)
- [Model Backends](#model-backends)
- [Operational Notes](#operational-notes)
- [Sources](#sources)

---

## What It Is

Summarize is a toolset for extracting clean text from links, files, and media and turning it into summaries. It’s available as a CLI and as a browser side panel/extension, and it’s positioned as useful for both agent workflows and humans (especially for YouTube/podcasts). [1] [2] [3]

Key links:
- Project site: `https://summarize.sh/` [2]
- Source and releases: `https://github.com/steipete/summarize` [3] [8]

## Interfaces

- **CLI (automation-friendly):** Accepts URLs and local files (including media), and supports modes like YouTube-specific extraction. [1] [3] [6]
- **Browser side panel:** A Chrome side panel (and Firefox sidebar) that summarizes the current tab and streams results, backed by a local daemon on `127.0.0.1` authenticated with a shared token. [3] [4]

## Offline-First and Caching

Summarize documents a local-first cache design intended to avoid repeated extraction/transcription/summarization work: a lightweight SQLite cache plus a separate on-disk media download cache for large assets. [5]

Notable details:
- **SQLite cache:** Defaults to `~/.summarize/cache.sqlite` with TTL/size caps and explicit CLI flags like `--no-cache` and `--clear-cache`. [5]
- **Media cache:** Defaults to `~/.summarize/cache/media` with a TTL and size cap, and can be disabled independently via `--no-media-cache`. [5]
- **Cache keys:** Designed to include inputs and settings (e.g., model/length/language) so changes don’t accidentally reuse stale results. [5]

## Media Pipeline (Transcripts and Fallbacks)

Summarize treats YouTube as “transcript-first” extraction, with multiple strategies and fallbacks depending on what the video exposes and what tools/keys are available. [6]

Examples of documented fallbacks:
- Multiple transcript extraction modes (`--youtube auto|web|no-auto|apify|yt-dlp`) with `yt-dlp` + transcription as a deeper fallback. [6]
- A recent release makes **Groq Whisper** the preferred cloud transcriber for reliability, with support for custom OpenAI-compatible Whisper endpoints. [8]

## Slides Mode (YouTube)

Summarize supports a “slides” extraction mode for YouTube videos, producing slide screenshots (and optional OCR) and writing outputs to a local directory. [1] [6]

## Model Backends

Summarize can route summarization calls through installed CLIs (including Codex, Claude, Gemini, and Cursor Agent) so it can work in “local CLI” setups without API keys, with configurable auto-fallback behavior. [7] [8]

## Operational Notes

The original announcement thread highlights a few pragmatic constraints and tradeoffs (some stated as intentions/claims):
- **Transcript gaps:** some inputs won’t have transcripts, so the pipeline needs fallbacks. [1] [6]
- **Local caching + “no phone home”:** described as caching locally and not phoning home. [1] [5]
- **Server-side caching caution:** described as potentially problematic (implied privacy/correctness risks). [1]
- **Network and performance constraints:** described as slow, and mentions a “residential IP” assumption. [1]

## Sources

1. [x.com (announcement thread)](https://x.com/i/status/2022513027870810384)
2. [summarize.sh](https://summarize.sh/)
3. [github.com/steipete/summarize](https://github.com/steipete/summarize)
4. [summarize.sh docs: chrome extension](https://summarize.sh/docs/chrome-extension.html)
5. [summarize.sh docs: cache](https://summarize.sh/docs/cache.html)
6. [summarize.sh docs: YouTube mode](https://summarize.sh/docs/youtube.html)
7. [summarize.sh docs: CLI models](https://summarize.sh/docs/cli.html)
8. [GitHub release v0.11.1 (includes v0.11.0 highlights)](https://github.com/steipete/summarize/releases/tag/v0.11.1)
