# CLAUDE.md for ExplorationMd

This file provides instructions for Claude when integrating new knowledge into this repository.

## Repository Structure

```
ExplorationMd/
├── logs/                    # Append-only category logs (chronological)
│   └── {category}.md        # e.g., programming.md, technology.md
├── corpus/                  # Integrated knowledge corpus (emergent structure)
│   ├── index.md             # Master index of all topics
│   └── {topic}/             # Topic directories
│       └── {subtopic}.md    # Integrated knowledge files
├── .meta/
│   └── sources.json         # Source tracking (v2 schema)
└── topics/                  # Legacy per-article files (read-only)
```

## Two-Tier Knowledge Organization

### 1. Category Logs (`logs/`)

Append-only chronological records. Each new URL creates an entry in the appropriate category log.

**Format:**
```markdown
### [Article Title](https://example.com/url)
*2026-02-08T10:30:00Z* | Tags: tag1, tag2

Summary paragraph describing the content.

**Key Points:**
  - First key insight
  - Second key insight

---
```

**Categories:** programming, technology, finance, science, health, misc

### 2. Knowledge Corpus (`corpus/`)

Emergent, integrated knowledge organized by topic. Content is synthesized and cross-referenced.

## Corpus File Structure

Every corpus file MUST have this structure:

```markdown
# Topic Title

## Index
- [Section One](#section-one)
- [Section Two](#section-two)

---

## Section One

Content with source references [1]...

## Section Two

More content [2]...

## Sources

1. [domain.com](https://full-url)
2. [another.com](https://another-url)
```

### Required Elements

1. **Title**: Single `#` heading at the top
2. **Index**: `## Index` section with anchor links to all content sections
3. **Separator**: `---` after the index
4. **Content Sections**: `##` headings for each topic area
5. **Sources Section**: `## Sources` at the end with numbered references

## Integration Guidelines

### When to Create New Files

- Create a new topic directory when content doesn't fit existing topics
- Create a new file within a topic for distinct subtopics
- Prefer updating existing files over creating new ones

### When to Update Existing Files

- New content relates to existing topic
- Content adds depth or new perspective to existing sections
- Content provides updated information

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

### Source References

- Use numbered references like `[1]` in text
- All factual claims should cite their source
- Add full URLs to the Sources section at the end

## Response Format for Integration

When integrating new content, respond with JSON only (no markdown fences):

```json
{
  "rationale": "Brief explanation of placement decision",
  "topic_path": "topic-name",
  "edits": [
    {
      "action": "create" | "update",
      "file_path": "topic-name/subtopic.md",
      "section": "Section Name" (for updates, null for creates),
      "content": "The markdown content",
      "sources": ["https://source-url"]
    }
  ],
  "index_updates": ["- [New Topic](topic-name/)"]
}
```

### Critical Rules for `content` Field

**For `"action": "update"`:**
- **DO NOT include `## Index`** - the system regenerates it automatically
- **DO NOT include `## Sources`** - sources are appended automatically
- **DO NOT include `---` separators** - the system manages file structure
- Only provide the actual section content (text, lists, code blocks)
- If updating a specific section, provide only that section's content (no heading)

**For `"action": "create"`:**
- Include the full file structure: title, content sections, `## Sources`
- **DO NOT include `## Index`** - it will be generated from your headings
- The system will add proper structure and separators
```

If content doesn't warrant corpus integration (too specific, ephemeral, or not generalizable):

```json
{
  "rationale": "Content is too specific for corpus integration",
  "topic_path": "",
  "edits": [],
  "index_updates": []
}
```

## Quality Guidelines

1. **Synthesize, don't copy**: Transform content into integrated knowledge
2. **Cross-reference**: Link related concepts across topics when relevant
3. **Maintain objectivity**: Present multiple perspectives fairly
4. **Be concise**: Distill to essential insights
5. **Use clear structure**: Consistent headings and formatting
6. **Cite sources**: Every factual claim needs a reference

## Topic Naming Conventions

- Use lowercase with hyphens: `agentic-software-development`
- Be specific but not overly narrow
- Group related content under common parent topics
