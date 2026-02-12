# AGENTS.md for ExplorationMd

Instructions for any agent when integrating new knowledge into this repository.

## Repository Structure

```
ExplorationMd/
├── README.md                    # Main entry point with links to all topics
├── AGENTS.md                    # These instructions
├── topics/                      # All knowledge organized by topic
│   └── {topic-name}/            # e.g., agentic-software-development
│       ├── README.md            # Topic overview with links to all articles
│       ├── logs.md              # Chronological log of sources for this topic
│       └── {article}.md         # Individual knowledge articles
└── .meta/
    └── sources.json             # Source tracking
```

## Navigation Principle

The repository is designed for markdown navigation. Every document should be reachable by clicking links starting from the main README.md.

## Topic Structure

Each topic is a directory under `topics/` containing:

### 1. README.md (Topic Overview)

```markdown
# Topic Name

Brief description of what this topic covers.

## Articles

- [Article Title](article-slug.md) - Brief description
- [Another Article](another-slug.md) - Brief description

## Recent Activity

See [logs.md](logs.md) for chronological source history.
```

### 2. logs.md (Chronological Log)

Append-only record of all sources for this topic.

```markdown
# Topic Name - Source Log

*Chronological record of sources for this topic*

---

### [Article Title](https://source-url)
*2026-02-09T10:30:00Z* | Tags: tag1, tag2

Summary paragraph describing the content.

**Key Points:**
  - First key insight
  - Second key insight

---
```

### 3. Article Files (Knowledge Articles)

Integrated knowledge synthesized from sources.

```markdown
# Article Title

## Index

- [Section One](#section-one)
- [Section Two](#section-two)
- [Sources](#sources)

---

## Section One

Content with source references [1]...

## Section Two

More content [2]...

## Sources

1. [domain.com](https://full-url)
2. [another.com](https://another-url)
```

## Integration Guidelines

### Determining Topic Placement

1. **Existing topic fits**: Add to that topic's logs.md and update/create articles
2. **New topic needed**: Create new topic directory with README.md and logs.md
3. **Topic naming**: Use lowercase with hyphens (e.g., `agentic-software-development`)

### When Processing New Content

1. **Always append to logs.md**: Every source gets a log entry in the topic's logs.md
2. **Integrate into articles**: Synthesize knowledge into new or existing article files
3. **Update README.md**: Add links to any new articles created

### Handling Conflicting Information

When sources disagree, present both views:

```markdown
### View A: [Perspective Name]
Source: [1]

Explanation of first perspective...

### View B: [Alternative Perspective]
Source: [2]

Explanation of alternative...

### Synthesis

Comparison of tradeoffs and when each approach applies...
```

## Quality Guidelines

1. **Synthesize, don't copy**: Transform content into integrated knowledge
2. **Cross-reference**: Link related concepts across topics when relevant
3. **Maintain objectivity**: Present multiple perspectives fairly
4. **Be concise**: Distill to essential insights
5. **Use clear structure**: Consistent headings and formatting
6. **Cite sources**: Every factual claim needs a reference
7. **Update READMEs**: Keep topic README.md files current with article links
