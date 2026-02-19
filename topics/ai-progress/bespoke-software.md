[← Back to Topic](README.md) | [← Home](../../README.md)

# Bespoke Software: The End of the App Store Model

As AI agents become capable of generating working software in minutes, the traditional model of discrete apps selected from an app store becomes increasingly obsolete. Instead, highly personalized, ephemeral applications can be generated on demand for specific use cases that would never justify a standalone product.

## Index

- [The Shift](#the-shift)
- [Example: A Custom Cardio Experiment Tracker](#example-a-custom-cardio-experiment-tracker)
- [Why App Stores Become Obsolete](#why-app-stores-become-obsolete)
- [AI-Native Services: Sensors and Actuators](#ai-native-services-sensors-and-actuators)
- [The Friction Gap: From One Hour to One Minute](#the-friction-gap-from-one-hour-to-one-minute)
- [Implications](#implications)
- [Sources](#sources)

---

## The Shift

The traditional software model assumes scarcity: building an application requires significant time and expertise, so users browse curated stores of pre-built solutions and adapt their workflows to available tools. AI-generated software inverts this: when a working application can be produced in minutes from a natural language description, the economics shift toward bespoke, single-purpose tools created for specific individuals and contexts [1].

## Example: A Custom Cardio Experiment Tracker

Karpathy describes building a personal fitness experiment tracker in one hour via "vibe coding" [1]:

- **Goal**: Lower resting heart rate from 50 to 45 BPM over 8 weeks using Zone 2 cardio minutes and weekly HIIT sessions
- **What the agent did**: Reverse-engineered the Woodway treadmill cloud API, pulled raw data, processed and filtered it, debugged issues (metric vs imperial units, calendar date matching), and built a web UI frontend
- **Result**: ~300 lines of code for a "super custom dashboard" tracking this specific experiment
- **Friction observed**: Not fully smooth—required noticing and directing bug fixes—but vastly faster than the ~10 hours this would have taken two years prior

The key observation: there will never be (and shouldn't be) a dedicated "Cardio experiment tracker" app on the App Store for this exact use case. The long tail of highly specific needs is better served by on-demand generation [1].

## Why App Stores Become Obsolete

The app store model assumes [1]:

1. **Discrete, pre-built products**: A finite catalog of solutions you browse and choose from
2. **General-purpose design**: Apps serve broad audiences, forcing users to adapt
3. **Download-install-use cycle**: Friction in finding, evaluating, installing, and learning new tools

AI-generated software breaks all three assumptions:

1. **Improvised on the spot**: The app is created specifically for your context
2. **Perfectly customized**: No compromises to serve other users
3. **Instant creation**: No browsing, no installation, no learning curve beyond a brief Q&A

The concept of an "app store" as a long tail of discrete apps becomes "somehow wrong and outdated when LLM agents can improvise the app on the spot and just for you" [1].

## AI-Native Services: Sensors and Actuators

For this model to reach its potential, services need to reconfigure as AI-native infrastructure [1]:

**Current state (2026)**:
- 99% of products/services lack AI-native CLI/API access
- Documentation remains in human-readable HTML/CSS meant for manual browsing
- Users are given webpage instructions to "open this URL and click here or there"
- Agents must reverse-engineer APIs that were designed for human frontends

**Required state**:
- Services expose sensors (physical state → digital knowledge) and actuators (digital commands → physical effects) via clean APIs/CLIs
- No human-readable frontend needed for agent consumption
- "My Woodway treadmill is a sensor... it shouldn't maintain some human-readable frontend and my LLM agent shouldn't have to reverse engineer it, it should be an API/CLI easily usable by my agent" [1]

This transition is happening slower than expected, and Karpathy notes this makes his adoption timelines correspondingly slower [1].

## The Friction Gap: From One Hour to One Minute

The current experience (one hour for a custom tracker) represents massive improvement over the past (~10 hours two years ago). But the exciting question is: what needs to be in place for this to take one minute? [1]

Requirements for the one-minute vision:
1. **Rich personal context**: The AI already knows relevant background about you
2. **Brief Q&A**: Minimal clarification needed to understand the goal
3. **Skill libraries**: Reference and search existing patterns/components
4. **Persistent orchestration**: Maintain all your little apps/automations over time

The future vision: "Simply say 'Hi can you help me track my cardio over the next 8 weeks', and after a very brief Q&A the app would be up" [1].

## Implications

- **Long tail unlocked**: Problems too specific for mass-market apps become solvable
- **App stores obsolete**: Browsing discrete pre-built apps becomes an anachronism
- **Service layer restructuring**: Pressure on every product to expose AI-native interfaces
- **Ephemeral software**: Applications created for short-term use, then discarded
- **Personal AI as orchestrator**: A single AI maintains your collection of bespoke tools

The summary: "The future are services of AI-native sensors & actuators orchestrated via LLM glue into highly custom, ephemeral apps. It's just not here yet." [1]

## Sources

1. [Andrej Karpathy on bespoke software (X post)](https://x.com/i/status/2024583544157458452)
