[← Back to Topic](README.md) | [← Home](../../README.md)

# Overnight Autonomous Agents

A system for making AI agents learn from daily sessions and ship features while you sleep.

## Index

- [The Concept](#the-concept)
- [The Two-Part Loop](#the-two-part-loop)
  - [Compound Review (10:30 PM)](#compound-review-1030-pm)
  - [Auto-Compound (11:00 PM)](#auto-compound-1100-pm)
- [Implementation](#implementation)
  - [Compound Review Script](#compound-review-script)
  - [Auto-Compound Script](#auto-compound-script)
  - [Scheduling with launchd](#scheduling-with-launchd)
  - [Keeping the Mac Awake](#keeping-the-mac-awake)
- [The Compound Effect](#the-compound-effect)
- [Debugging](#debugging)
- [Extension Ideas](#extension-ideas)
- [Dependencies](#dependencies)
- [Sources](#sources)

---

## The Concept

Most developers use AI agents reactively—prompt, response, move on. An overnight automation loop changes this pattern: the agent reviews the day's work, extracts lessons, updates its own instructions, and then picks up the next feature from a backlog. All while you sleep.

The key insight is that AGENTS.md (or CLAUDE.md) becomes a living knowledge base that grows every night. The agent reads its own updated instructions before each implementation run, creating a self-improving feedback loop.

## The Two-Part Loop

The system runs two sequential jobs nightly. Order matters.

### Compound Review (10:30 PM)

Reviews all threads from the last 24 hours, extracts learnings, and updates AGENTS.md files. This captures patterns and gotchas that were discovered during the day but not explicitly compounded.

### Auto-Compound (11:00 PM)

Pulls latest from main (now with fresh context from the review job), picks the #1 priority from prioritized reports, implements it, and creates a draft PR.

The sequencing ensures implementation benefits from the day's learnings.

## Implementation

### Compound Review Script

```bash
#!/bin/bash
# scripts/daily-compound-review.sh
# Runs BEFORE auto-compound.sh to update AGENTS.md with learnings

cd ~/projects/your-project

# Ensure we're on main and up to date
git checkout main
git pull origin main

amp execute "Load the compound-engineering skill. Look through and read each Amp thread from the last 24 hours. For any thread where we did NOT use the Compound Engineering skill to compound our learnings at the end, do so now - extract the key learnings from that thread and update the relevant AGENTS.md files so we can learn from our work and mistakes. Commit your changes and push to main."
```

**For Claude Code**, replace the `amp execute` line with:

```bash
claude -p "Load the compound-engineering skill. Look through and read each Claude thread from the last 24 hours. For any thread where we did NOT use the Compound Engineering skill to compound our learnings at the end, do so now - extract the key learnings from that thread and update the relevant CLAUDE.md files so we can learn from our work and mistakes. Commit your changes and push to main." --dangerously-skip-permissions
```

What the agent does:
1. Finds all threads from the last 24 hours
2. Checks if each thread ended with a compound step
3. For threads that didn't, retroactively extracts learnings
4. Updates relevant AGENTS.md/CLAUDE.md files
5. Commits and pushes to main

### Auto-Compound Script

```bash
#!/bin/bash
# scripts/compound/auto-compound.sh
# Full pipeline: report → PRD → tasks → implementation → PR

set -e
cd ~/projects/your-project

# Source environment
source .env.local

# Fetch latest (including tonight's AGENTS.md updates)
git fetch origin main
git reset --hard origin/main

# Find the latest prioritized report
LATEST_REPORT=$(ls -t reports/*.md | head -1)

# Analyze and pick #1 priority
ANALYSIS=$(./scripts/compound/analyze-report.sh "$LATEST_REPORT")
PRIORITY_ITEM=$(echo "$ANALYSIS" | jq -r '.priority_item')
BRANCH_NAME=$(echo "$ANALYSIS" | jq -r '.branch_name')

# Create feature branch
git checkout -b "$BRANCH_NAME"

# Create PRD
amp execute "Load the prd skill. Create a PRD for: $PRIORITY_ITEM. Save to tasks/prd-$(basename $BRANCH_NAME).md"

# Convert to tasks
amp execute "Load the tasks skill. Convert the PRD to scripts/compound/prd.json"

# Run the execution loop
./scripts/compound/loop.sh 25

# Create PR
git push -u origin "$BRANCH_NAME"
gh pr create --draft --title "Compound: $PRIORITY_ITEM" --base main
```

The pipeline:
1. Pulls main with fresh learnings
2. Finds the latest prioritized report
3. Picks the #1 priority item
4. Creates a feature branch
5. Generates a PRD (Product Requirements Document)
6. Converts PRD to actionable tasks
7. Runs the execution loop (iterates until all tasks pass or hits limit)
8. Creates a draft PR

### Scheduling with launchd

macOS launchd handles edge cases better than cron. Create plist files for each job.

**Compound Review (10:30 PM):**

```xml
<!-- ~/Library/LaunchAgents/com.yourproject.daily-compound-review.plist -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.yourproject.daily-compound-review</string>

  <key>ProgramArguments</key>
  <array>
    <string>/Users/you/projects/your-project/scripts/daily-compound-review.sh</string>
  </array>

  <key>WorkingDirectory</key>
  <string>/Users/you/projects/your-project</string>

  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>22</integer>
    <key>Minute</key>
    <integer>30</integer>
  </dict>

  <key>StandardOutPath</key>
  <string>/Users/you/projects/your-project/logs/compound-review.log</string>

  <key>StandardErrorPath</key>
  <string>/Users/you/projects/your-project/logs/compound-review.log</string>

  <key>EnvironmentVariables</key>
  <dict>
    <key>PATH</key>
    <string>/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin</string>
  </dict>
</dict>
</plist>
```

**Auto-Compound (11:00 PM):**

```xml
<!-- ~/Library/LaunchAgents/com.yourproject.auto-compound.plist -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.yourproject.auto-compound</string>

  <key>ProgramArguments</key>
  <array>
    <string>/Users/you/projects/your-project/scripts/compound/auto-compound.sh</string>
  </array>

  <key>WorkingDirectory</key>
  <string>/Users/you/projects/your-project</string>

  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>23</integer>
    <key>Minute</key>
    <integer>0</integer>
  </dict>

  <key>StandardOutPath</key>
  <string>/Users/you/projects/your-project/logs/auto-compound.log</string>

  <key>StandardErrorPath</key>
  <string>/Users/you/projects/your-project/logs/auto-compound.log</string>

  <key>EnvironmentVariables</key>
  <dict>
    <key>PATH</key>
    <string>/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin</string>
  </dict>
</dict>
</plist>
```

Load the jobs:

```bash
launchctl load ~/Library/LaunchAgents/com.yourproject.daily-compound-review.plist
launchctl load ~/Library/LaunchAgents/com.yourproject.auto-compound.plist
```

Verify:

```bash
launchctl list | grep yourproject
```

### Keeping the Mac Awake

launchd won't wake a sleeping Mac. Use caffeinate to keep it awake during the automation window:

```xml
<!-- ~/Library/LaunchAgents/com.yourproject.caffeinate.plist -->
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>Label</key>
  <string>com.yourproject.caffeinate</string>
  <key>ProgramArguments</key>
  <array>
    <string>/usr/bin/caffeinate</string>
    <string>-i</string>
    <string>-t</string>
    <string>32400</string>
  </array>
  <!-- Start at 5pm, keep awake for 9 hours (until 2am) -->
  <key>StartCalendarInterval</key>
  <dict>
    <key>Hour</key>
    <integer>17</integer>
    <key>Minute</key>
    <integer>0</integer>
  </dict>
</dict>
</plist>
```

This keeps the Mac awake from 5 PM to 2 AM—plenty of time for nightly jobs to complete.

## The Compound Effect

What happens every night:

1. **10:30 PM** - Agent reviews day's threads, finds missed learnings, updates AGENTS.md, pushes to main
2. **11:00 PM** - Agent pulls main (now with fresh context), picks top priority, implements it, opens a PR

When you wake up:
- Updated AGENTS.md files with patterns the agent learned
- A draft PR implementing your next priority
- Logs showing exactly what happened

The agent gets smarter every day because it reads its own updated instructions before each implementation run. Patterns discovered on Monday inform Tuesday's work. Gotchas hit on Wednesday are avoided on Thursday.

## Debugging

Check if jobs are scheduled:

```bash
launchctl list | grep yourproject
```

View logs:

```bash
tail -f ~/projects/your-project/logs/compound-review.log
tail -f ~/projects/your-project/logs/auto-compound.log
```

Test manually:

```bash
launchctl start com.yourproject.daily-compound-review
```

## Extension Ideas

- **Slack notifications** when PRs are created or jobs fail
- **Multiple priority tracks** - run different reports on different nights
- **Automatic PR merge** if CI passes and changes are small
- **Weekly summary** - have the agent write a changelog of everything it shipped

## Dependencies

Three open-source projects power this workflow:

1. **[Compound Engineering Plugin](https://github.com/EveryInc/compound-engineering-plugin)** - Skill for extracting and persisting learnings from sessions
2. **[Compound Product](https://github.com/snarktank/compound-product)** - Automation layer turning prioritized reports into shipped PRs (includes auto-compound.sh, execution loop, PRD-to-tasks pipeline)
3. **[Ralph](https://github.com/snarktank/ralph)** - Autonomous agent loop that runs continuously until tasks are complete

## Sources

1. [substack.com](https://substack.com)
