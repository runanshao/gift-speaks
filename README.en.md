# 礼说 / gift-speaks

> Let your gift speak for you.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skill](https://img.shields.io/badge/Claude-Skill-blue.svg)](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)

Languages: [中文](README.md) | [English](README.en.md) | [日本語](README.ja.md)

## What is this

A Claude skill. It helps you choose a gift that **actually lands** for the person you're in an intimate relationship with — not a trending gift list, not AI clichés, but 3 personalized "gift + message + how to give it" combos built from **private details in your own notes**.

**Core belief: the purpose of a gift is to deepen trust between two people.**

## 30-second overview

![Hero screenshot](demo/hero-screenshot.png)

Input:
> "It's my wife's birthday next week. We just moved and she's exhausted."
> + your notes folder (or paste a few excerpts)

Output (3 plans, each with 5 fields):
- 🎁 Specific gift (model / price / where to buy / delivery time)
- ✉️ What to say when you give it
- 🕯️ When and how to present it
- 🗝️ Why this plan works — citing which private detail from your notes
- 💬 If they ask "how did you think of this?", here's what you can say

Plus: a clear recommendation ranking so you don't spiral into decision paralysis.

## Who it's for

✅ Use it when:
- You want to give a gift to a partner / crush / someone you love, but you're stuck
- You roughly know what they're into, but can't translate that into a concrete gift + message
- You're in a repair moment and want to show sincerity without misfiring

❌ Don't use it for:
- Gifts to managers / clients / colleagues (completely different emotional logic)
- Gifts to parents or siblings (different relational context)
- "AI-curated trending gift list" (that's not what this does)

## How to use

1. Clone this repo:
   ```bash
   git clone https://github.com/runanshao/gift-speaks.git
   ```

2. Copy to Claude's skills directory:
   ```bash
   # macOS / Linux
   cp -r gift-speaks ~/.claude/skills/gift-speaks

   # Windows (PowerShell)
   Copy-Item -Recurse gift-speaks "$env:USERPROFILE\.claude\skills\gift-speaks"
   ```

3. Start a new conversation in Claude Code

4. Just describe your situation:
   > "It's my wife's birthday next week, we just moved and she's exhausted..."

5. The skill activates automatically, talks with you briefly, then gives you 3 plans

6. Not happy? Say "swap #2 / too expensive / rewrite the message" — iterate freely

## Privacy

- Your notes are analyzed entirely locally — **nothing is uploaded to the cloud**
- By default the skill reads only the last 6 months; you can specify keyword filters
- You can exclude any keywords you don't want read (e.g., an ex's name)
- Nothing is quoted verbatim — only "distilled details" are referenced

## Examples

- [examples/birthday-busy-wife.md](examples/birthday-busy-wife.md) — Birthday + exhausted wife
- [examples/apology-cold-war.md](examples/apology-cold-war.md) — Cold war + apology (with crisis reminder)
- [examples/support-stress.md](examples/support-stress.md) — She's under pressure, I feel for her
- [examples/no-material-fallback.md](examples/no-material-fallback.md) — What if I have no notes?
- [examples/iteration-flow.md](examples/iteration-flow.md) — How to iterate if you're not happy

## Design philosophy

- **The gift is the vehicle; trust is the destination**
- **Private details beat any abstract framework** (psychology backbones like Gary Chapman's 5 love languages exist, but a concrete detail always wins over theory)
- **AI provides the plan; the user owns the words** (talking-points are written so you can say them in your own voice)
- **In crisis scenarios, a gift is support — not a substitute** (cold war / apology situations get an explicit reminder)
- **3-round conversation cap — this isn't a survey**

Full design doc: [docs/superpowers/specs/2026-05-25-gift-speaks-design.md](docs/superpowers/specs/2026-05-25-gift-speaks-design.md)

## What it won't do

- ❌ Gift shopping links (URLs go stale — search keywords last longer)
- ❌ Cross-session memory / cloud accounts (privacy first)
- ❌ Multi-agent orchestration (one skill handles it; no over-engineering)
- ❌ Business / family / friendship gift scenarios (focused on one thing done well)

## License

MIT — see [LICENSE](LICENSE).

## Contributing

Issues and PRs welcome. When modifying prompts, please include before/after conversation samples — prompts aren't regular code; you need to show actual behavioral change to validate any edit.
