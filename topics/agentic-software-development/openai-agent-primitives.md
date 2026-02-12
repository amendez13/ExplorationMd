[← Back to Topic](README.md) | [← Home](../../README.md)

# OpenAI Agent Primitives: Skills, Shell, and Compaction

OpenAI introduced three agentic primitives for building long-running agents: Skills (reusable instruction bundles), Shell Tool (hosted execution environments), and Server-Side Compaction (automatic context management). These patterns apply broadly to any agent framework.

## Index

- [Skills as Procedures](#skills-as-procedures)
- [Skills API Implementation](#skills-api-implementation)
  - [SKILL.md Manifest Structure](#skillmd-manifest-structure)
  - [Creating Skills via API](#creating-skills-via-api)
  - [Using Skills in API Calls](#using-skills-in-api-calls)
  - [Practical Example: CSV Insights](#practical-example-csv-insights)
  - [Limits and Validation](#limits-and-validation)
- [Shell Execution Environment](#shell-execution-environment)
- [Context Management via Compaction](#context-management-via-compaction)
- [Ten Practical Tips](#ten-practical-tips)
- [Implementation Patterns](#implementation-patterns)
- [Recommended Workflow](#recommended-workflow)
- [Sources](#sources)

---

## Skills as Procedures

Skills bundle files with a `SKILL.md` manifest, functioning as versioned playbooks the model consults when needed [1]. The platform exposes skill name, description, and path to inform model routing decisions.

This aligns with the Agent Skills open standard—a common format across providers.

Skills occupy a distinct position in the agent architecture [2]:

| Component | Purpose |
|-----------|---------|
| **System prompts** | Global behavior, safety boundaries, always-on principles |
| **Tools** | External services, side effects, live state fetching |
| **Skills** | Packaged procedures with code, scripts, and assets for conditional execution |

Skills are ideal for:
- Reusable, independently versionable workflows
- Complex conditional logic with branching requirements
- Procedures requiring code execution and local artifacts
- Keeping system prompts minimal
- Shared organizational standards across agents

Skills are less suitable for one-off tasks (inline scripts suffice), live external data requirements (use tools instead), or constantly changing procedures.

---

## Skills API Implementation

### SKILL.md Manifest Structure

Every skill requires a `SKILL.md` manifest with YAML frontmatter [2]:

```
skill_folder/
├── SKILL.md (required, includes frontmatter)
├── scripts (*.py, *.js, etc.)
├── requirements.txt
└── assets/
```

The manifest frontmatter:

```yaml
---
name: skill-name
description: Brief purpose and use cases
---
```

When attached to hosted shell environments, the system:
- Uploads and unzips skill bundles into runtime
- Extracts metadata from frontmatter
- Adds skill name, description, and path to system context
- Allows models to invoke skills by reading manifests and executing scripts

### Creating Skills via API

**Option A: Multipart file upload**

```bash
curl -X POST 'https://api.openai.com/v1/skills' \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F 'files[]=@./skill_name/SKILL.md;filename=skill_name/SKILL.md' \
  -F 'files[]=@./skill_name/script.py;filename=skill_name/script.py'
```

**Option B: Zip upload (recommended)**

```bash
curl -X POST 'https://api.openai.com/v1/skills' \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -F 'files=@./skill_name.zip;type=application/zip'
```

Zips are portable, easy to version, and a useful workaround when uploads misbehave [2].

### Using Skills in API Calls

**Hosted shell (container_auto):**

```python
from openai import OpenAI
client = OpenAI()

response = client.responses.create(
  model="gpt-5.2",
  tools=[{
    "type": "shell",
    "environment": {
      "type": "container_auto",
      "skills": [
        {"type": "skill_reference", "skill_id": "<skill_id>"},
        {"type": "skill_reference", "skill_id": "<skill_id>", "version": 2},
      ],
    },
  }],
  input="Use the skills to analyze the uploaded CSV."
)
```

**Local shell:**

```python
response = client.responses.create(
    model="gpt-5.2",
    tools=[{
        "type": "shell",
        "environment": {
            "type": "local",
            "skills": [
                {"type": "skill_reference", "skill_id": "<skill_id>"},
            ],
        },
    }],
    input="Use the configured skills locally."
)
```

Use explicit versions in production (e.g., `version: 2`) rather than floating to `"latest"` [2].

### Practical Example: CSV Insights

A complete skill for CSV analysis:

```python
import argparse
from pathlib import Path
import pandas as pd

def write_report(df: pd.DataFrame, outpath: Path) -> None:
    lines = [f"# CSV Insights Report\n"]
    lines.append(f"**Rows:** {len(df)}  \n**Columns:** {len(df.columns)}\n")
    numeric = df.select_dtypes(include="number")
    if not numeric.empty:
        lines.append("\n## Numeric summary\n")
        lines.append(numeric.describe().to_markdown())
    outpath.write_text("\n".join(lines), encoding="utf-8")

def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("--input", required=True)
    parser.add_argument("--outdir", required=True)
    args = parser.parse_args()
    df = pd.read_csv(args.input)
    write_report(df, Path(args.outdir) / "report.md")
```

CLI design principles [2]:
- Scripts should run from command line
- Print deterministic output
- Fail with clear errors
- Write to known paths

### Limits and Validation

- Maximum zip size: 50 MB
- Maximum files per skill: 500
- Maximum uncompressed file size: 25 MB
- Case-insensitive manifest matching (skill.md or SKILL.md)
- Exactly one manifest required per skill [2]

---

## Shell Execution Environment

The shell tool operates either through OpenAI-managed containers or locally-controlled runtimes [1]. Key characteristics:

- Enables dependency installation, script execution, and output generation
- Integrated with the Responses API for stateful work and multi-turn continuation
- Container reuse supported for continuity across turns
- `/mnt/data` serves as the artifact boundary for outputs to retrieve or pass downstream

---

## Context Management via Compaction

Compaction preserves continuity during extended workflows through automatic in-stream compression or explicit `/responses/compact` endpoint usage [1].

This addresses the fundamental constraint of context windows filling during long-running tasks—similar to Claude Code's auto-compaction and `/compact` command.

---

## Ten Practical Tips

### 1. Skill Descriptions as Routing Logic

Write descriptions answering: when to use, when to avoid, outputs, and success criteria—not marketing language [1].

### 2. Negative Examples Reduce Misfires

Include explicit "don't call when..." scenarios. Early Glean testing showed ~20% initial accuracy drops that recovered after adding edge case coverage [1].

### 3. Embed Templates Inside Skills

Templates and examples loaded only upon invocation don't inflate unrelated queries, improving latency and quality for structured outputs [1].

### 4. Design for Continuity Early

Use container reuse, pass `previous_response_id`, and treat compaction as a default rather than emergency fallback [1].

### 5. Explicit Determinism

When production workflows require predictability, instruct models: "Use the [skill name] skill" [1].

### 6. Secure Skill + Network Combinations

Network access combined with powerful procedures creates exfiltration risks. Maintain strict allowlists and assume tool output is untrusted [1].

### 7. Use `/mnt/data` as Artifact Boundary

Designate this directory for outputs retrieved, reviewed, or passed downstream [1].

### 8. Two-Layer Allowlist System

Organization-level allowlists set maximum destinations; request-level policies must remain subsets. Requests exceeding org allowlists error [1].

### 9. `domain_secrets` for Authentication

Models see credential placeholders; a sidecar injects real values only for approved destinations, preventing credential exposure [1].

### 10. Consistent API Across Environments

Skills work with both hosted and local shell modes, enabling development iteration locally before production deployment [1].

---

## Implementation Patterns

### Pattern A: Data Pipeline

Install dependencies → fetch external data → write artifact to `/mnt/data` [1]

### Pattern B: Deterministic Workflows

Encode workflows in skills, mount into shell environment, execute deterministically for spreadsheet analysis, dataset cleaning, or standardized reports [1].

### Pattern C: Enterprise SOPs (Glean Example)

Skills as living SOPs encoding recurring tasks. Glean achieved:
- 73% → 85% accuracy improvements
- 18.1% time-to-first-token reductions

Using Salesforce-oriented skills [1].

---

## Recommended Workflow

1. Iterate locally during development
2. Transition to hosted containers for repeatability
3. Maintain skills consistently across deployment contexts
4. Keep networking locked with minimal allowlists
5. Use domain secrets for authenticated API calls

---

## Sources

1. [Shell + Skills + Compaction: Tips for Long-Running Agents](https://developers.openai.com/blog/skills-shell-tips) - OpenAI Developer Blog, February 2026
2. [Skills in OpenAI API - Cookbook](https://developers.openai.com/cookbook/examples/skills_in_api) - OpenAI Developer Documentation
