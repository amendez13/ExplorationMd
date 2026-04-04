[← Back to Topic](README.md) | [← Home](../../README.md)

# LLM Knowledge Bases

A workflow for building personal knowledge bases where an LLM serves as both compiler and query engine. Rather than manually organizing notes, the LLM incrementally transforms raw source materials into an interconnected wiki of markdown files.

## Index

- [The Architecture](#the-architecture)
- [Data Ingest](#data-ingest)
- [Wiki Compilation](#wiki-compilation)
- [The Obsidian IDE](#the-obsidian-ide)
- [Q&A at Scale](#qa-at-scale)
- [Output Formats](#output-formats)
- [Linting and Enhancement](#linting-and-enhancement)
- [Custom Tooling](#custom-tooling)
- [The "LLM Domain" Principle](#the-llm-domain-principle)
- [Future Direction: Finetuning](#future-direction-finetuning)
- [Sources](#sources)

---

## The Architecture

The system has three layers:

1. **Raw sources**: Articles, papers, repos, datasets, images indexed into a `raw/` directory
2. **Compiled wiki**: A nested directory of `.md` and `.png` files maintained entirely by the LLM
3. **Derived outputs**: Slide shows, visualizations, and query results rendered on demand

The human adds sources manually and asks questions. The LLM handles everything else: summarization, categorization, cross-linking, and incremental enhancement.

## Data Ingest

Sources are collected into a `raw/` directory. The process is not fully autonomous—each source is added manually, one by one, with human oversight especially in early stages.

Tools for ingestion:

- **Obsidian Web Clipper**: Browser extension that converts web articles into `.md` files
- **Image download hotkey**: Downloads related images to local storage so the LLM can reference them directly

After adding enough sources, the LLM "gets" the pattern and marginal documents become easier to add: "file this new doc to our wiki: (path)".

## Wiki Compilation

The LLM incrementally builds the wiki from raw sources:

1. **Summaries**: Brief summaries of all documents in `raw/`
2. **Categorization**: Data organized into concepts
3. **Articles**: Standalone articles written for each concept
4. **Cross-linking**: Backlinks connecting related concepts

The wiki schema is kept simple and flat—a nested directory of `.md` files, `.png` images, and occasionally `.csv` and `.py` files. The schema is documented in an `AGENTS.md` file that the LLM can reference.

## The Obsidian IDE

Obsidian serves as the "IDE frontend" for the knowledge base:

- View raw source data
- Navigate the compiled wiki
- Render derived visualizations
- Preview slide shows (via Marp plugin)

The key insight: Obsidian is purely for viewing and navigation. The human rarely touches the wiki files directly—editing is the LLM's job.

## Q&A at Scale

Once the wiki reaches sufficient scale (e.g., ~100 articles, ~400K words), it becomes queryable:

- Ask complex questions against the wiki
- LLM researches answers across the knowledge base
- Results incorporate information from multiple sources

At this scale, no fancy RAG infrastructure is needed. The LLM auto-maintains index files and brief summaries, reading relevant data fairly easily. This "small scale" approach works because the LLM can follow links and summaries to locate what it needs.

## Output Formats

Instead of getting answers in text/terminal, outputs are rendered as files viewable in Obsidian:

- **Markdown files**: Structured answers and reports
- **Slide shows**: Marp-format presentations
- **Matplotlib images**: Visualizations and charts

A key workflow: outputs often get "filed" back into the wiki, enhancing it for future queries. Personal explorations and queries "add up" in the knowledge base over time.

## Linting and Enhancement

LLM "health checks" run periodically over the wiki to:

- Find inconsistent data
- Impute missing data (using web searchers)
- Discover interesting connections for new article candidates
- Suggest further questions to investigate

These checks incrementally clean up the wiki and enhance its overall data integrity.

## Custom Tooling

Additional tools can be developed to process the data. Example: a small, naive search engine over the wiki that provides both:

- **Web UI**: For direct human use
- **CLI**: For the LLM to use as a tool when handling larger queries

This pattern—building tools the LLM can invoke—extends the system's capabilities without requiring RAG complexity.

## The "LLM Domain" Principle

The core philosophy: the wiki is the domain of the LLM, not the human.

> "You rarely ever write or edit the wiki manually, it's the domain of the LLM."

The human's role is:
- Adding source materials
- Asking questions
- Reviewing outputs
- Occasionally filing outputs back into the wiki

This division of labor plays to each party's strengths.

## Future Direction: Finetuning

As the repository grows, the natural desire is to have the LLM "know" the data in its weights instead of just in context windows:

- **Synthetic data generation**: Create training examples from the wiki
- **Finetuning**: Train a model that has internalized the knowledge base

This would enable faster queries and reduce context window constraints, though it adds complexity and maintenance overhead.

## Sources

1. [x.com/karpathy](https://x.com/karpathy/status/2039805659525644595) - Andrej Karpathy's thread on LLM knowledge bases
