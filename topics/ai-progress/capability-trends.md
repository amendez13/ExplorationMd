[← Back to Topic](README.md) | [← Home](../../README.md)

# AI Capability Trends

## Index

- [Historical Perspective](#historical-perspective)
- [Capability Timeline](#capability-timeline)
- [Productivity Compression](#productivity-compression)
- [Enterprise Adoption Signals](#enterprise-adoption-signals)
- [METR Task Duration Measurements](#metr-task-duration-measurements)
- [Cost Dynamics](#cost-dynamics)
- [Model Recommendations](#model-recommendations)
- [Sources](#sources)

---

## Historical Perspective

The current AI moment gains context when viewed against the full arc of computing history. Eric S. Raymond, who began programming in 1975, provides a firsthand account of the transformation [3]:

**1975 Computing**:
- Programs run via punched cards fed into programmable calculators
- Computers were "giant creatures that lived in glass-walled rooms"
- Unix and C had not yet left Bell Labs; DOS and IBM PC were six years away
- Global computing capacity roughly equivalent to a single modern smartphone
- Teletypes used as production equipment; video terminals barely existed
- Pixel-addressable color displays were science fiction
- No version control, no public forges, computer games countable on two hands

**Then vs Now**: Programming was "slow and laborious"—monthly code output was tiny by modern standards. Today, practitioners converse with AI to produce "finished programs I would once have considered prohibitively complex to attempt within a single working day" [3].

Even science-fiction fans couldn't have predicted this timeline. Raymond notes he wouldn't have predicted pocket devices with real-time access to world knowledge arriving within "multiple centuries" [3].

## Capability Timeline

The pace of AI improvement has been dramatic [2]:

- **2022**: AI couldn't do basic arithmetic reliably (would confidently state 7 × 8 = 54)
- **2023**: Passed the bar exam
- **2024**: Could write working software and explain graduate-level science
- **Late 2025**: Top engineers reported handing over most coding work to AI
- **February 5, 2026**: GPT-5.3 Codex and Opus 4.6 released, marking another step change

The key insight: AI focused on coding first because code-writing ability accelerates AI development itself. Making AI great at coding was the strategy that unlocks everything else [2].

## Productivity Compression

Developers using frontier AI models report transformational productivity gains. Tasks that previously required weeks can be completed in hours with AI assistance [1].

Current experience from practitioners [2]:
- Describing desired app in plain English, walking away for 4 hours, returning to find it built
- AI writes tens of thousands of lines of code that work correctly
- AI opens the app, clicks through buttons, tests features, iterates on its own
- AI demonstrates something resembling judgment and taste—knowing "what the right call is"

**Concrete metrics from Claude Code creator** [4]:
- 30-day output: 259 PRs, 497 commits, 40,000 lines added, 38,000 lines removed
- Every line written by Claude Code + Opus 4.5 with no human-written code
- AI runs for minutes, hours, and even days autonomously (using Stop hooks)
- One year prior, the same AI struggled with basic bash escaping and worked only seconds to minutes at a time

**Solo developer vs large team equivalence** [6]:
- Peter Steinberger's OpenClaw project demonstrates that "one standout dev going all-in on sensible AI usage can out-ship a team of 50+ engineers" according to Gergely Orosz (The Pragmatic Engineer)
- The pattern is clearest for specific use cases: rewriting existing software with high test coverage yields ~100x efficiency gains
- Important variable: test suite coverage is decisive—software without tests is harder and riskier to rewrite with AI assistance
- This creates a new incentive dynamic: protecting and hiding actual test suites may become a competitive concern

## Enterprise Adoption Signals

Signals of AI-driven productivity gains are also starting to appear in large, mature product organizations. In Spotify's Q4 earnings call, co-CEO Gustav Söderström said the company's best developers "have not written a single line of code since December" as internal tooling and Claude Code take over much of the direct code production [5]. Spotify described an internal system ("Honk") that enables remote, real-time development and deployment workflows—for example, asking Claude (via Slack on a phone) to fix a bug or add an iOS feature during a commute, then reviewing the result and merging to production before arriving at the office [5].

If accurate and repeatable, this suggests a role shift where top engineers spend more time on specifying, reviewing, and shipping changes than on manual typing—while retaining responsibility for correctness and product decisions [5].

## METR Task Duration Measurements

METR tracks the length of real-world tasks (measured by human expert completion time) that AI can complete successfully end-to-end without human help [2]:

- ~1 year ago: ~10 minutes
- Then: ~1 hour
- Then: several hours
- November 2025 (Claude Opus 4.5): ~5 hours

**Doubling rate**: Approximately every 7 months, with recent data suggesting acceleration to as fast as every 4 months.

If trend continues:
- Days of independent work: within ~1 year
- Weeks of independent work: within ~2 years
- Month-long projects: within ~3 years

## Cost Dynamics

Token costs for frontier models don't drop significantly at any given capability level. However, to achieve the same level of intelligence, costs decrease substantially over time.

**Pattern**: Today's "150 IQ" model costs X per token. Next year, "150 IQ" tokens will be significantly cheaper, but a new "200 IQ" model will be available at roughly the same X price point [1].

Result: costs continuously drop for equivalent capability, but the ceiling keeps rising.

## Model Recommendations

For best results as of early 2026:
- **ChatGPT**: GPT-5.2 Pro or GPT-5.3 Codex [1, 2]
- **Claude**: Opus 4.5 or Opus 4.6 [1, 2]

Important: Default models in free tiers are often a year or more behind what paying users access. Select the most capable model explicitly in settings [2].

## Sources

1. [X thread on AI progress](https://x.com/i/status/2021256989876109403)
2. [Something Big Is Happening - Matt Shumer](https://x.com/mattshumer_)
3. [Fifty Years of Progress - Eric S. Raymond](https://x.com/esrtweet/status/2004829013068444050)
4. [Claude Code Creator Reflects on One Year of Progress - Boris Cherny](https://x.com/i/status/2004887829252317325)
5. [TechCrunch: Spotify says its best developers haven't written a line of code since December, thanks to AI](https://techcrunch.com/2026/02/12/spotify-says-its-best-developers-havent-written-a-line-of-code-since-december-thanks-to-ai/)
6. [Solo Developer Outshipping Large Teams - Gergely Orosz](https://x.com/i/status/2027019191044173874)
