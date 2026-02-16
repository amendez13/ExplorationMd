[← Back to Topic](README.md) | [← Home](../../README.md)

# Long-Term Time Tracking

Lessons from 11+ years and 44,000 rows of time data, revealing how objective measurement corrects perception biases and creates an unexpected life diary.

## Index

- [The Origin Story](#the-origin-story)
- [The Schema](#the-schema)
- [The Perception Gap](#the-perception-gap)
- [Hidden Time Sinks](#hidden-time-sinks)
- [The Monotony Heuristic](#the-monotony-heuristic)
- [The Accidental Diary](#the-accidental-diary)
- [Tool Evolution](#tool-evolution)
- [Sources](#sources)

---

## The Origin Story

Pablo Santos Luaces began tracking time in 2014 while managing Plastic SCM, a version control system. The motivation was validation—he wanted to measure software estimation accuracy. But there was a deeper problem: he felt unproductive despite working long hours, suspecting interruptions were fragmenting his focus.

As he puts it: "I wanted to move away from just sensations and into real data."

This impulse wasn't new. Santos traces it to childhood, when he built a bicycle-tracking program in the 1990s, inspired by *Doogie Howser, M.D.*'s nightly journaling. The pattern—measuring and documenting change—eventually shaped his entire career in version control.

## The Schema

The system's longevity came from intentional simplicity:

| Field | Purpose |
|-------|---------|
| Start time | When the activity began |
| End time | When the activity ended |
| Type | Category (work, personal, etc.) |
| Subtype | Specific activity within category |
| Notes | Free-form context |

Five fields. No complexity. The minimal structure ensured sustainability—anything more elaborate would have been abandoned. The original implementation used Excel/Google Sheets with manual pivot table analysis.

## The Perception Gap

The first major revelation: estimated work hours didn't match reality.

Santos believed he worked 6-9 hours daily. The data showed **3.5 hours of focused work**, fragmented across the day by interruptions.

This wasn't laziness—it was visibility into how context switching and interruptions consume time invisibly. The sensation of "working all day" masked the actual productive output buried within it.

## Hidden Time Sinks

Aggregated data exposed patterns invisible to intuition. A 2018 snapshot revealed:

| Category | % of Work Time | Hours |
|----------|----------------|-------|
| Marketing | 15.4% | 290 |
| Unproductive | 4.3% | - |
| Email | 3.2% | - |

Marketing consumed far less time than Santos perceived—but the perception mismatch itself was the insight. Without data, he would have continued overestimating some activities and underestimating others.

The unproductive and email percentages revealed organizational inefficiencies: time that felt productive but didn't advance meaningful work.

## The Monotony Heuristic

A counterintuitive pattern emerged over years:

> "Fewer entries = better day."

Weeks with one dominant project—monotonous on paper—generated more output than busy weeks with multiple context switches. The busy weeks *felt* productive (more variety, more meetings, more activities logged) but produced less.

This aligns with deep work research: sustained focus on a single problem outperforms fragmented attention across many problems, even when the latter feels more engaging.

## The Accidental Diary

The notes field was an afterthought—a place to record brief context about what was happening during each time block.

Over 11 years, those notes accumulated into a searchable life diary. Santos could query past decisions, reconstruct the emotional context of events, and gain retrospective perspective impossible through memory alone.

This was an unintended benefit. The system designed for productivity measurement became a tool for self-understanding.

## Tool Evolution

The manual Excel/Sheets approach worked but required periodic analysis effort. Santos eventually built **rows.life**, a dedicated time tracker incorporating:

- Built-in journaling integration
- Zero-knowledge encryption (privacy by design)
- Full data export capabilities
- Automated analytics

The product reflects the core lessons: simple entry schema, notes as first-class citizens, and ownership of your own data.

## Sources

1. [I've Tracked 44,000 Rows of My Life Since 2014 - Pablo Santos Luaces](https://psantosl.github.io/posts/16-years-of-tracking/)
