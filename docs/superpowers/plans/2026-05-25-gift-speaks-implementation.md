# Gift-Speaks Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the gift-speaks Claude skill — an open-source markdown-based skill that helps users in intimate relationships choose meaningful gifts + write resonant messages, by mining their personal notes for private details and outputting 3 ranked, ready-to-purchase gift+message combos. North Star: increase trust between people.

**Architecture:** A self-contained Claude skill: one entry `SKILL.md` (frontmatter + orchestration), modular prompt fragments in `prompts/`, scripted dialogue examples in `examples/`, demo media in `demo/`, and trilingual READMEs. No code, no dependencies — pure prompt engineering on Claude's built-in Read/Glob tools.

**Tech Stack:** Markdown, Git, Claude Code as test harness, MIT license. Optional: ScreenToGif (Windows) for demo recording.

**Source spec:** `C:\Users\runan\.claude\plans\skill-zazzy-lollipop.md` (will be migrated into the project's `docs/superpowers/specs/` during Task 3)

---

## File Structure

Project root: chosen in Task 1. Two candidates:
- `D:\runanproject\gift-speaks\` (matches CLAUDE.md vision, D: drive not yet created)
- `C:\Users\runan\gift-speaks\` (lives on C: where everything else is)

All paths below are relative to `<PROJECT_ROOT>/`.

```
gift-speaks/
├── README.md                                  # Chinese main README
├── README.en.md                               # English README
├── README.ja.md                               # Japanese README
├── SKILL.md                                   # Skill entry (frontmatter + orchestration)
├── LICENSE                                    # MIT
├── .gitignore
├── prompts/
│   ├── greeting.md                            # Round 0: warm opening + 3 examples
│   ├── round1-parsing.md                      # Scene parsing rules
│   ├── round2-followup.md                     # Smart follow-up + optional light fields
│   ├── round3-locate.md                       # Locate-confirm with crisis hook
│   ├── crisis-warning.md                      # Anti-misuse reminder text
│   ├── psych-framework.md                     # Love languages / attachment / MBTI
│   ├── generate-3plans.md                     # 3×5-field generation + certainty rules
│   ├── self-review.md                         # Internal "is this actually moving?" check
│   └── iterate.md                             # Round 4+ iteration handlers
├── examples/
│   ├── birthday-busy-wife.md                  # Happy + material
│   ├── apology-cold-war.md                    # Crisis (with reminder)
│   ├── support-stress.md                      # Tender / 心疼
│   ├── no-material-fallback.md                # No-material honest fallback
│   └── iteration-flow.md                      # Round 4+ adjustment
├── demo/
│   ├── demo.gif                               # 1-2 minute screencast
│   ├── hero-screenshot.png                    # "30-second understanding" image
│   └── wow-moment.png                         # The moment a user sees a戳人方案
└── docs/
    └── superpowers/
        ├── specs/
        │   └── 2026-05-25-gift-speaks-design.md
        └── plans/
            └── 2026-05-25-gift-speaks-implementation.md
```

### Per-file responsibility

- `SKILL.md` — single entry Claude loads. Frontmatter declares the skill; body orchestrates the multi-round flow by pointing at `prompts/*.md` fragments at the appropriate moments.
- `prompts/*.md` — behavioral fragments. Each is a focused instruction set Claude follows at one specific moment. Pure prose, kept short (< 200 lines each) for reliable in-context loading.
- `examples/*.md` — complete user↔assistant transcripts. Triple purpose: in-context exemplars for the skill, README demonstration material, manual regression tests.
- `README*.md` — user-facing project description. Marketing + onboarding.
- `demo/` — visual assets that decide whether GitHub visitors install or scroll past.

---

## Tasks

### Task 1: Pick project location and create directory structure

**Files:**
- Create: `<PROJECT_ROOT>/` and all subdirectories

- [ ] **Step 1: Ask user to choose project root**

Ask: "Two options for project location — `D:\runanproject\gift-speaks\` (matches CLAUDE.md, D: drive not yet present, will need to create both) or `C:\Users\runan\gift-speaks\` (already-present C: drive)? Or specify another path."

Wait for explicit answer. Record the chosen path. From here on, `<PROJECT_ROOT>` refers to this path.

- [ ] **Step 2: Create directory tree**

Run (PowerShell, substitute `<PROJECT_ROOT>` with the chosen path):

```powershell
$root = "<PROJECT_ROOT>"
New-Item -ItemType Directory -Force -Path "$root","$root\prompts","$root\examples","$root\demo","$root\docs\superpowers\specs","$root\docs\superpowers\plans" | Out-Null
Get-ChildItem -Recurse $root | Select-Object FullName
```

Expected: 6 directories listed, all under the chosen root.

- [ ] **Step 3: Commit the empty structure with a `.gitkeep` in each empty dir**

```powershell
"" | Out-File -Encoding utf8 "$root\prompts\.gitkeep"
"" | Out-File -Encoding utf8 "$root\examples\.gitkeep"
"" | Out-File -Encoding utf8 "$root\demo\.gitkeep"
"" | Out-File -Encoding utf8 "$root\docs\superpowers\specs\.gitkeep"
"" | Out-File -Encoding utf8 "$root\docs\superpowers\plans\.gitkeep"
```

(Git commit happens in Task 2 after `git init`.)

---

### Task 2: Initialize git, write LICENSE and .gitignore

**Files:**
- Create: `<PROJECT_ROOT>/LICENSE`
- Create: `<PROJECT_ROOT>/.gitignore`

- [ ] **Step 1: `git init` and configure**

```bash
cd <PROJECT_ROOT>
git init
git branch -M main
```

Expected: "Initialized empty Git repository in ..."

- [ ] **Step 2: Write `.gitignore`**

Create `<PROJECT_ROOT>/.gitignore` with content:

```
# OS
.DS_Store
Thumbs.db
desktop.ini

# Editors
.vscode/
.idea/
*.swp
*~

# Node (in case anyone adds tooling later)
node_modules/
npm-debug.log*

# Python (in case)
__pycache__/
*.pyc
.venv/

# Local notes the user might drop in for testing
local-notes/
test-vault/
*.local.md
```

- [ ] **Step 3: Write `LICENSE` (MIT)**

Create `<PROJECT_ROOT>/LICENSE` with content (replace `<YEAR>` with `2026` and `<COPYRIGHT_HOLDER>` with `runan`):

```
MIT License

Copyright (c) 2026 runan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

- [ ] **Step 4: First commit**

```bash
cd <PROJECT_ROOT>
git add .
git commit -m "chore: init repo with scaffolding, MIT license, gitignore"
```

Expected: commit succeeds with around 7-8 files (the `.gitkeep`s, LICENSE, .gitignore).

---

### Task 3: Migrate the design spec into the project

**Files:**
- Create: `<PROJECT_ROOT>/docs/superpowers/specs/2026-05-25-gift-speaks-design.md`
- Create: `<PROJECT_ROOT>/docs/superpowers/plans/2026-05-25-gift-speaks-implementation.md`
- Source: `C:\Users\runan\.claude\plans\skill-zazzy-lollipop.md`
- Source: `C:\Users\runan\.claude\plans\2026-05-25-gift-speaks-implementation.md` (this file)

- [ ] **Step 1: Copy spec into project**

```powershell
Copy-Item "C:\Users\runan\.claude\plans\skill-zazzy-lollipop.md" `
  "<PROJECT_ROOT>\docs\superpowers\specs\2026-05-25-gift-speaks-design.md"
```

- [ ] **Step 2: Copy this implementation plan into project**

```powershell
Copy-Item "C:\Users\runan\.claude\plans\2026-05-25-gift-speaks-implementation.md" `
  "<PROJECT_ROOT>\docs\superpowers\plans\2026-05-25-gift-speaks-implementation.md"
```

- [ ] **Step 3: Remove the `.gitkeep`s in those two dirs**

```powershell
Remove-Item "<PROJECT_ROOT>\docs\superpowers\specs\.gitkeep"
Remove-Item "<PROJECT_ROOT>\docs\superpowers\plans\.gitkeep"
```

- [ ] **Step 4: Commit**

```bash
cd <PROJECT_ROOT>
git add docs/
git commit -m "docs: import design spec and implementation plan"
```

---

### Task 4: Write SKILL.md skeleton (frontmatter + orchestration)

**Files:**
- Create: `<PROJECT_ROOT>/SKILL.md`

- [ ] **Step 1: Write the complete SKILL.md**

Create `<PROJECT_ROOT>/SKILL.md` with this content. This file is the single entry Claude reads when activating the skill. It declares the skill metadata and the high-level flow; details live in `prompts/`.

````markdown
---
name: gift-speaks
description: Help the user choose a meaningful gift + write a resonant message for someone in their intimate relationship (partner / spouse / crush). Mines the user's personal notes for private details that turn a generic gift into one that says "you see me". Use when the user expresses any of: wanting to give a gift, not knowing what to give, wanting to write a card/message for a partner, navigating a relationship moment (birthday, anniversary, apology, support). Triggers in any language; replies in the user's language. NOT FOR business gifting, gifts to family/friends/colleagues — intimate relationships only.
---

# 礼说 / gift-speaks

> 来，让你的礼物为你说话。
> Let your gift speak for you.
> あなたの想いを、ギフトに語らせて。

## North Star

**The purpose of a gift is to deepen trust between two people.**
Every decision in this skill — what to ask, what to recommend, how to phrase — is judged by one question: *does this strengthen trust, or weaken it?*

## When to activate

The user is in an intimate relationship (romantic partner / spouse / crush) and is approaching a moment where a gift + message would matter: birthday, anniversary, apology, support, reconciliation, celebration, "just because".

Do NOT activate for: business gifting, gifts to parents/family/colleagues/friends, generic copywriting requests.

## Flow

Follow this orchestration. Each `prompts/<name>.md` file contains the detailed instructions for that step.

```
Round 0 → prompts/greeting.md
   Open warmly. Show 3 example situations (different emotional tones).
   Ask the user to describe their situation.

Round 1 → prompts/round1-parsing.md
   Parse the user's message. Identify:
     - relationship role (do not assume gender or orientation)
     - urgency / timing
     - emotional tone
     - whether this is a CRISIS scenario (apology / cold war / breakup repair)
   Decide: is the information sufficient to proceed?

Round 2 (conditional) → prompts/round2-followup.md
   Only if Round 1 information is insufficient.
   Ask at most 1-2 follow-ups, framed as memory triggers, not survey questions.
   Optionally absorb: budget, past gifts (to avoid repeats), MBTI if user mentions it.

Round 3 → prompts/round3-locate.md
   Mirror the user's situation back in your own words.
   If CRISIS scenario, attach prompts/crisis-warning.md content here.
   Get explicit confirmation before generating plans.

Material gathering
   Three modes (user picks):
     - Path: user gives a folder path; you Read/Glob for relevant files
       (apply time-window and keyword filters per prompts/round2-followup.md)
     - Paste: user pastes notes inline
     - None: honest fallback — proceed on scene description only,
       tell the user quality will be reduced; never fabricate details
   Show the privacy notice on first path-mode use (see prompts/round2-followup.md).

Generation → prompts/psych-framework.md + prompts/generate-3plans.md
   Apply the priority order:
     1. Private details (decisive)
     2. Scene signals (strong)
     3. Love language inference (medium)
     4. Relationship stage (reference)
     5. MBTI if user provided it (tiebreaker)
   Output 3 plans × 5 fields each (gift / message / timing / basis / talking-point),
   then state recommendation ranking with reasoning.
   Gift fields MUST include model/brand/price/channel/delivery info.

Self-review → prompts/self-review.md
   Before showing output to the user, silently grade each plan.
   If any plan reads as generic or unsupported by the gathered material,
   rewrite that plan once.

Iteration → prompts/iterate.md
   After output, support free-form iteration requests:
     - replace one plan
     - re-generate with different constraints (cheaper / louder / softer)
     - tweak only the message of plan N
     - add a new plan in a specific style
   Reuse the material analysis; do not re-interrogate.
```

## Language

Reply in the language the user wrote to you in. If they switch languages mid-conversation, switch with them. Don't ask which language — detect and adapt.

## What I will never do

- Recommend specific external purchase URLs (recommend search keywords + channels instead, since URLs go stale)
- Read notes outside the user-specified filter (time window / keywords)
- Upload anything to the cloud — all material analysis is local within this session
- Fabricate "private details" if I didn't find any in the material — I'll say so honestly
- Push the user to spend more than they hinted at
- Treat crisis scenarios (apology / cold war) as ordinary gift moments — see prompts/crisis-warning.md

````

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add SKILL.md
git commit -m "feat: add SKILL.md orchestration entry"
```

---

### Task 5: Manual smoke test — verify Claude picks up the skill

This is a manual verification step. There's no automation; we just need to know the skill activates.

- [ ] **Step 1: Make the skill discoverable to Claude Code locally**

Two options (pick one):

A. Copy the project into Claude's user skills directory:
```powershell
$dest = "$env:USERPROFILE\.claude\skills\gift-speaks"
New-Item -ItemType Directory -Force -Path $dest | Out-Null
Copy-Item -Recurse -Force "<PROJECT_ROOT>\*" $dest
```

B. Symlink (preferred — edits in repo are seen live):
```powershell
$dest = "$env:USERPROFILE\.claude\skills\gift-speaks"
New-Item -ItemType SymbolicLink -Force -Path $dest -Target "<PROJECT_ROOT>"
```

(Symlink requires Developer Mode enabled on Windows, or running PowerShell as admin.)

- [ ] **Step 2: Open a fresh Claude Code session and check the skill list**

In a new Claude Code session, ask: "List the skills available to you."

Expected: `gift-speaks` appears in the list with its description.

- [ ] **Step 3: Trigger the skill with a test prompt**

Send: "我想给我老婆挑个生日礼物，下周生日"

Expected: Claude activates the gift-speaks skill (you should see the greeting from SKILL.md, even though the prompts/ files aren't filled in yet — the greeting is currently a "TODO" but the orchestration is loaded). The point of this step is just confirming activation, not the full flow.

If the skill doesn't activate, check the SKILL.md frontmatter description (it must contain trigger phrases like "gift", "送礼", "送什么").

- [ ] **Step 4: Commit any frontmatter fixes**

If you tuned the description for better triggering, commit:

```bash
cd <PROJECT_ROOT>
git add SKILL.md
git commit -m "tune: improve SKILL.md description for activation"
```

---

### Task 6: Write prompts/greeting.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/greeting.md`

- [ ] **Step 1: Write the file**

Create `<PROJECT_ROOT>/prompts/greeting.md` with content:

```markdown
# Round 0: Greeting

Output this exact greeting (translate naturally to the user's detected language; keep tone identical):

---

嗨～你来这里，说明心里有个 TA。

跟我说说你现在的处境吧——可以是任何关于 TA 和你的事。
你不用想得太完整，第一反应告诉我就行。

🌰 比如：
  · "下周是我老婆生日，刚搬完家她特别累，想给她个惊喜"
  · "和女朋友冷战三天，因为我忘了她妈手术那天，想道歉"
  · "她最近考研压力很大，我心疼她"

我会跟你聊几句，然后给你 3 套礼物 + 文案 + 怎么送的方案。

---

## Tone rules

- Warm, never gushy
- Empathetic, never therapist-y
- Direct about what comes next ("we'll talk briefly, then I give you 3 plans")
- Never start with "Of course!" / "I'd be happy to!" / "As an AI..."

## Example variations (translate, don't transliterate)

English version:
> Hey — you're here, which means there's someone on your mind.
> Tell me what's going on. Anything about you two, doesn't have to be polished. First thought is fine.
>
> 🌰 For example:
>   · "It's my wife's birthday next week, we just moved, she's exhausted"
>   · "We've been in a cold war 3 days — I forgot her mom's surgery date, I want to apologize"
>   · "She's drowning in exam prep, I just feel for her"
>
> We'll talk for a moment, then I give you 3 gift + message + timing plans.

Japanese version:
> こんにちは。ここに来たということは、心に大切な人がいるんですね。
> その人と、いまの状況を聞かせてください。何でもいい、まとまっていなくて構いません。
>
> 🌰 たとえば：
>   · 「来週は妻の誕生日、引っ越したばかりで彼女は疲れきっている」
>   · 「彼女と三日冷戦中、お義母さんの手術日を忘れてしまって謝りたい」
>   · 「彼女が受験勉強で参っている、何かしてあげたい」
>
> 少し話を聞いて、それからギフト・メッセージ・渡し方の3案をお出しします。

## What to do after sending

Wait for the user's response. Then move to Round 1 (prompts/round1-parsing.md).
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/greeting.md
git commit -m "feat: add greeting prompt with multilingual examples"
```

---

### Task 7: Write prompts/round1-parsing.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/round1-parsing.md`

- [ ] **Step 1: Write the file**

```markdown
# Round 1: Scene Parsing

After the user describes their situation, extract these four signals silently. Do not show this analysis to the user.

## Extract

1. **Relationship role** — who is the recipient?
   Pull from the user's wording: "wife / husband / girlfriend / boyfriend / partner / 老婆 / 老公 / 女朋友 / 男朋友 / 喜欢的人 / 妻 / 彼女 / etc."
   **Do NOT assume gender or sexual orientation.** If unclear, ask in Round 2; never default to "she".

2. **Urgency / timing** — when does the gift need to land?
   "Tomorrow / next week / in a month / no specific date" → categorize as `urgent (< 3 days)`, `soon (1-2 weeks)`, `relaxed (> 2 weeks)`, `unscheduled`.
   This affects delivery-time recommendations for the gift.

3. **Emotional tone** — what's the user trying to express?
   `celebration / appreciation / tenderness (心疼) / longing / apology / reconciliation / playful / "just because"`.
   Pick one or two dominant tones.

4. **Crisis flag** — is this a relationship-repair scenario?
   Trigger keywords (any language): cold war / argued / broke up / apology / "I was wrong" / sorry / mad at me / cheating / trust crisis / divorce / repair / win back / 冷战 / 吵架 / 分手 / 道歉 / 我错了 / 对不起 / 她生气 / 出轨 / 挽回.
   If matched → set `crisis = true`. This activates prompts/crisis-warning.md in Round 3.

## Decide: is information sufficient?

You have enough if you can answer all four signals confidently AND you have at least a hint about *why* this moment matters (the specific situation, not just "her birthday").

- Sufficient → skip Round 2, go straight to Round 3 (prompts/round3-locate.md).
- Insufficient → go to Round 2 (prompts/round2-followup.md).

## What "insufficient" looks like

- "I want to give my girlfriend a gift" → no occasion, no context → insufficient.
- "Birthday gift for my wife" → role + occasion but no specific context → insufficient (ask for one concrete detail).
- "Wife's birthday next week, we just moved, she's exhausted" → all four signals + a real situation → sufficient.
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/round1-parsing.md
git commit -m "feat: add round-1 scene parsing rules"
```

---

### Task 8: Write prompts/round2-followup.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/round2-followup.md`

- [ ] **Step 1: Write the file**

```markdown
# Round 2: Smart Follow-up (conditional)

Triggered only if Round 1 marked the information as insufficient. Otherwise skip.

## Hard rules

- Maximum 2 questions. Prefer 1.
- Each question must trigger a **specific memory**, not request a category.
- Frame as conversation, not as a form.

## Memory-trigger questions (pick the most relevant)

- "她最近最让你心疼的瞬间是什么？第一反应就行。"
- "你想到 TA 时，最近脑子里浮现的画面是什么？"
- "TA 上次跟你抱怨什么事？哪怕特别小的事。"
- "你们最近一次让你觉得'她真的累了'的对话是什么时候？"
- (English) "What's the most recent moment that hit you about her?"
- (English) "What did she complain about lately, even something tiny?"

## What NOT to ask

- ❌ "她的爱好是什么？" — too survey-y, returns generic answers
- ❌ "她喜欢什么品牌？" — same
- ❌ "你们关系怎么样？" — too abstract

## Optional light absorption (don't ask, just absorb if user volunteers)

If during the conversation the user mentions any of these naturally, store them silently:

- **Budget**: any number / range. If absent, the 3 plans default to a price gradient (low / mid / high) so budget is implicitly covered.
- **Past gifts** (last 1-2 years): used to filter out repeats in generate-3plans.
- **MBTI**: only if user volunteers ("she's INFJ"). Never ask "what's her MBTI?" — that breaks the warm tone.

## Privacy notice (first time path-mode is mentioned)

If the user is about to give you a folder path (Obsidian vault, notes folder), output this notice FIRST, before reading anything:

```
在我开始读你的笔记之前，告诉你几件事：

· 我只读你指定范围的文件（默认最近 6 个月）
· 我可以按关键词过滤——比如只读包含 TA 名字的笔记
· 我可以排除你指定的关键词——比如前任名字
· 整个过程在你电脑本地，我不会把任何笔记内容传到云端
· 我只引用"提炼后的细节"，不会原文转述

请告诉我：
  1. 文件夹路径
  2. 时间窗口（默认 6 个月，也可以说"最近一年/三个月"）
  3. 必读关键词（如 TA 的名字）
  4. 排除关键词（可选）
```

Translate to the user's language. Wait for their answer before reading.

## After Round 2

Go to Round 3 (prompts/round3-locate.md).
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/round2-followup.md
git commit -m "feat: add round-2 follow-up with memory-trigger questions and privacy notice"
```

---

### Task 9: Write prompts/round3-locate.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/round3-locate.md`

- [ ] **Step 1: Write the file**

```markdown
# Round 3: Locate Confirmation

This is the "I see you" moment. Mirror back your understanding of the user's situation in your own words. This step is non-negotiable.

## Output format

Two paragraphs:

1. **Mirror** — restate the situation in your own voice. Pull in 1-2 specific signals from Round 1.

   Example:
   > "我理解你的处境是：下周老婆生日，但你们刚搬完家，她正在那种'连自己都顾不上'的疲惫状态。
   >  你想做的，不是热闹的庆祝，是让她感觉'有人替我顾着我'。"

2. **Confirmation question** — close-ended.
   > "是这样吗？如果是，我去找你笔记里相关的细节，然后给你 3 套方案。"

## CRISIS branch

If Round 1 set `crisis = true`, append the full content of prompts/crisis-warning.md AFTER the mirror, BEFORE the confirmation question. The flow becomes:

```
Mirror →
Crisis warning →
Confirmation question
```

The user must confirm before plans are generated.

## After user confirms

- Trigger material gathering (path / paste / none) per SKILL.md flow.
- Then go to prompts/psych-framework.md + prompts/generate-3plans.md.

## If user disagrees with the mirror

- Apologize briefly, ask for the one piece you got wrong, re-mirror.
- Do not re-run Round 2 — just patch the misunderstanding and re-confirm.
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/round3-locate.md
git commit -m "feat: add round-3 locate confirmation with crisis hook"
```

---

### Task 10: Write prompts/crisis-warning.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/crisis-warning.md`

- [ ] **Step 1: Write the file**

```markdown
# Crisis Scenario Anti-misuse Reminder

Inserted between the mirror and the confirmation question in Round 3, IF and only if `crisis = true`.

## Output (translate to user's language, keep voice identical)

```
我注意到这是个『关系修复』场景。说几句真心话——

**礼物只是辅助，不是道歉的替代品。** TA 真正需要的，可能不是一份礼物，而是：
  · 你认真承认错了什么（具体到事，不要泛泛而谈）
  · 你说清楚下次怎么不重蹈覆辙
  · 给 TA 情绪空间，不要急于求"原谅"

礼物可以是诚意的载体，但只在"你已经把上面三件事做了"之后才有用。

接下来我会给你 3 套方案，会偏克制、低调，避免"用钱解决感情问题"的观感。
```

## Effect on plan generation

When `crisis = true` is set, prompts/generate-3plans.md applies these additional constraints:

- All 3 plans skew SMALL, PRIVATE, "she/he personally needs this" — never showy
- Message field elements REQUIRED: acknowledgment + no-defense + give-space + no-demand-for-forgiveness
- Timing field MUST specify "after a genuine apology conversation, not as substitute"
- Talking-point field MUST avoid "I'm sorry" alone — pair with what changes going forward
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/crisis-warning.md
git commit -m "feat: add crisis-scenario anti-misuse reminder"
```

---

### Task 11: Write prompts/psych-framework.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/psych-framework.md`

- [ ] **Step 1: Write the file**

```markdown
# Psychology Framework (internal, never shown to user)

Used by prompts/generate-3plans.md as input to plan-shape decisions.

## Priority of factors (descending — upper rows always win)

| # | Factor | Source | Note |
|---|---|---|---|
| 1 | **Private details** | User's material (notes / paste) | Decisive. A concrete detail beats every abstract framework below. |
| 2 | **Scene signals** | Round 1 parsing | Birthday → celebration tone; apology → restraint; tender → small/personal |
| 3 | **Love language** | Inferred from material + scene | Medium weight. See table below. |
| 4 | **Relationship stage** | Inferred from material + Round 1 | Crush / dating / stable / repair — adjusts gift weight |
| 5 | **MBTI** | Only if user volunteered | Tiebreaker only |

**Concrete beats abstract.** If MBTI/love-language analysis suggests gift A, but the material says she wanted B, give B.

## Love language inference (Gary Chapman)

Look in material/scene for cues:

| If you find... | Likely love language | Plan-shape implication |
|---|---|---|
| "She loves it when you say..." / verbal appreciation moments | Words of affirmation | Heavy on the message; the gift can be modest |
| "We never get enough time together" / scheduling complaints | Quality time | Recommend an experience > an object |
| "She mentions wanting X" / unwrapping moments | Receiving gifts | Object quality + packaging + ceremony matters |
| "I just need help with X" / overwhelm complaints | Acts of service | A done-for-her task beats a thing — e.g., "I'll handle dinner all week" |
| Physical comfort / touch references | Physical touch | Tactile gifts win — scarves, throws, soft items |

Default if unclear: pick a mix across the 3 plans, then refine.

## Attachment style (hidden — never reveal label to user)

| Cue in material | Likely style | Plan adjustment |
|---|---|---|
| Wants reassurance often, fear-of-loss language | Anxious | Add reassurance into the message field; gift can be repeating-presence (something used daily) |
| "Doesn't like fuss" / shrugs off attention | Avoidant | Smaller gift, message gives space, no demand for response |
| Mix of warmth and pulling-away | Disorganized | Plan B (safe) recommended; avoid grand gestures |
| Stable warmth + clear communication | Secure | Any plan works; use private-detail strength as the differentiator |

## MBTI (optional, tiebreaker only)

If the user said "she's INFJ" / similar:
- N-types → meaning/symbolism > shiny object
- S-types → concrete, sensory, well-made
- T-types → useful, well-engineered
- F-types → personal, story-laden
- J-types → planned, structured (book the experience in advance)
- P-types → spontaneous, flexible (gift card to a place they love)

Never ASK for MBTI. Use only if user said it.
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/psych-framework.md
git commit -m "feat: add internal psychology framework (love languages, attachment, MBTI)"
```

---

### Task 12: Write prompts/generate-3plans.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/generate-3plans.md`

- [ ] **Step 1: Write the file**

```markdown
# Generate 3 × 5-field Plans

Triggered after Round 3 confirmation + material gathering. Produces the final user-visible output.

## Output structure (exactly 3 plans, each with exactly 5 fields)

Use this exact layout. Translate field labels to user's language; preserve emoji and structure.

```
💎 方案一 · 细水长流款

🎁 礼物：[具体到型号/品牌/关键词，含价位、渠道、到货时间]

✉️ 文案：[1 句到 1 段]

🕯️ 时机 & 呈现：[何时、用什么方式送]

🗝️ 依据（仅供你参考）：[引用素材里哪条私密细节，或说明为何这个组合契合]

💬 如果 TA 问"你怎么想到的"，你可以这样说：
   [一句让用户能"内化"成自己说法的话术]

💎 方案二 · 惊喜爆破款
（同 5 字段）

💎 方案三 · 安全稳妥款
（同 5 字段）

---

🎯 我最推荐：方案 X，因为 [具体理由，引用素材或场景信号]。
   如果你担心 [具体顾虑]，那就选方案 Y。
   方案 Z 是兜底——简单不出错，但戳人程度有限。
```

## Three-plan differentiation

These three lanes must be CLEARLY different — not three variations of the same idea.

- **Plan 1 · 细水长流款** (steady / low-key) — daily usable, modest price, "I see you in the everyday"
- **Plan 2 · 惊喜爆破款** (high-drama / memorable) — bigger gesture, higher price, "I see something specific about you", has some risk of overshooting
- **Plan 3 · 安全稳妥款** (safe / dignified) — middle price, quality + tact, low-risk, "I respect you"

The three plans naturally form a price gradient even if the user didn't state a budget.

## Hard requirements on the 🎁 Gift field

EVERY gift field MUST include:

- Specific model / brand / search keyword (so the user can paste it into Taobao / JD / Amazon)
- Price range in user's currency (or "¥200-400" / "$30-50" / "¥3000-5000円" style)
- Purchase channel (Taobao? Xiaohongshu? Brand website? Amazon Japan? Etsy?)
- Delivery time hint ("next-day delivery available" / "needs 3-5 day order ahead" / "order at least a week in advance")

❌ Forbidden: "尤加利配香薰" (vague)
✅ Required: "尤加利干花束（小红书搜'尤加利桉树叶'，¥80-150，顺丰次日达）+ Lumira 黑无花果香薰（天猫海外旗舰店，¥380，预计 5-7 天到货）"

If the user is in a different country/currency context, adapt channels (Etsy / Amazon JP / Rakuten / etc).

## Hard requirements on the 🗝️ Basis field

This field justifies the recommendation. It MUST do ONE of:

- Quote a specific private detail from the material with a rough date if available
  → "基于你 2025-09-12 日记里她抱怨办公室味道"
- If no material, explain which scene signal drove the choice
  → "基于你说'刚搬完家她特别累'——搬家季的疲惫提示她需要小确幸而非大场面"

NEVER fabricate a detail. If material yielded nothing, say so honestly here.

## Hard requirements on the 💬 Talking-point field

This is what the USER can say if TA asks "你怎么想到的". It must:

- Sound like the user said it, NOT like AI wrote it
- Reference the underlying observation, not the AI's reasoning
- Be ≤ 2 sentences

✅ "最近一直留意你的状态，发现你提过几次办公室那股味道。"
❌ "AI analyzed your notes and identified an opportunity to address her olfactory environment."

## Certainty principle

After the 3 plans, ALWAYS state a recommendation ranking. Format:

```
🎯 我最推荐：方案 X
   理由：[一句话，引用具体素材或场景信号]
   备选：方案 Y（如果你担心 [具体担忧]）
   兜底：方案 Z（不出错但平淡）
```

NEVER:
- "三个都挺好，看你喜欢哪个" — decision-paralysis, forbidden
- "也许方案 1，可能方案 2" — vague hedging, forbidden

ALLOWED hedging (specific, actionable):
- "方案 1 适合 TA 不抗拒香味的情况——如果你不确定 TA 对香味的接受度，那直接选方案 2。"

## CRISIS adjustments

If `crisis = true` from Round 1:

- All 3 plans skew small, private, restrained (no public gestures)
- Message field MUST contain: acknowledgment + no defense + give space + no demand for forgiveness
- Recommendation usually points to Plan 3 (安全稳妥) — least risk of looking like "buying forgiveness"
- Add a footer: "记住，礼物是诚意的载体，不是道歉本身。"

## After output

Hand off to prompts/self-review.md BEFORE showing to user.
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/generate-3plans.md
git commit -m "feat: add 3-plan generation with 5-field structure and certainty rules"
```

---

### Task 13: Write prompts/self-review.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/self-review.md`

- [ ] **Step 1: Write the file**

```markdown
# Internal Self-Review (silent, before output)

After generating 3 plans but BEFORE showing them to the user, run this check silently.

## Grading questions (per plan)

For each of the 3 plans, ask yourself:

1. **Does the gift cite a specific private detail or scene signal?** (Y/N)
2. **Does the message sound like the user, not like AI?** (Y/N — look for: clichés, "may this gift bring you..." style, vague affirmations)
3. **Is the gift field purchasable?** (model/brand/price/channel/delivery all present? Y/N)
4. **Is the timing field specific?** (a concrete moment, not "soon" or "when convenient")
5. **Can the user actually say the talking-point out loud without sounding fake?** (Y/N)

## Fail criteria

A plan FAILS review if it has ≥ 2 "N" answers, OR if it scores N on question 2 (the AI-tone check is the strictest gate).

## Repair action

For each failed plan: rewrite it ONCE silently.

- Keep the lane (细水长流 / 惊喜爆破 / 安全稳妥)
- Find the weakest field (most likely 文案 or 依据) and rewrite it
- Re-grade. If it still fails, downgrade honesty: change the 依据 field to "基于你的场景描述，没有从素材里挖到具体细节" — better honest weak than confidently fake.

## After repair

Output all 3 plans + the recommendation ranking to the user. Self-review is invisible to them.
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/self-review.md
git commit -m "feat: add internal self-review with 5-question grading"
```

---

### Task 14: Write prompts/iterate.md

**Files:**
- Create: `<PROJECT_ROOT>/prompts/iterate.md`

- [ ] **Step 1: Write the file**

```markdown
# Round 4+: Iteration

After the initial 3 plans are shown, the user can iterate freely. Reuse all prior analysis. Do not re-interrogate.

## Supported iteration patterns

| User says... | Action |
|---|---|
| "第二个不喜欢，再换一个" / "I don't like #2, swap it" | Replace Plan 2 only, keep 1 and 3. Re-grade the new Plan 2 via self-review. |
| "整体太贵了 / 太轻了 / 太煽情了" | Regenerate ALL 3 plans with the new constraint as an added rule. |
| "再加一个 [style] 的方案" | Add a 4th plan in that style. Update recommendation ranking. |
| "把第一个的文案改一下，更 [adjective]" | Rewrite only the message field of Plan 1. Keep other fields. |
| "把购买链接告诉我" | Politely decline — explain that URLs go stale; offer to convert the gift descriptor into a copy-pasteable search query for the user's preferred platform. |
| "礼物挺好，再帮我写一段短信发给她" | Generate a stand-alone short message based on the recommended plan. Apply same anti-AI-tone rules. |

## Hard rules

- Do not ask the user to re-describe the situation
- Do not re-run Round 2 follow-ups unless the user introduces NEW context (e.g., "actually, she just told me she..." — that's new material, treat as Round 1 addition)
- Always re-state the updated recommendation at the end of the iteration

## End of iteration signals

When the user says any of:
- "好的我用方案 X"
- "OK I'll go with this"
- "够了谢谢"
- "thanks"

Close warmly without sycophancy. Single short line. Examples:

> "祝顺利。送出去那一刻，你已经做对了一半。"
> "Hope it lands. The fact that you thought this hard means you're already most of the way there."

Do NOT:
- Ask if they want more plans
- Offer follow-up services
- Add disclaimers
- Wish them luck in 3 paragraphs
```

- [ ] **Step 2: Commit**

```bash
cd <PROJECT_ROOT>
git add prompts/iterate.md
git commit -m "feat: add round-4+ iteration handlers with anti-sycophancy ending"
```

---

### Task 15: Integration smoke test — run end-to-end

This is a manual session test, not code.

- [ ] **Step 1: Re-sync skill to Claude's skills directory if not symlinked**

If you used the copy approach in Task 5, re-copy:
```powershell
Copy-Item -Recurse -Force "<PROJECT_ROOT>\*" "$env:USERPROFILE\.claude\skills\gift-speaks\"
```

If you symlinked, no action needed.

- [ ] **Step 2: Open a fresh Claude Code session**

Start a new session (so prior context doesn't leak).

- [ ] **Step 3: Run scenario A — Happy + paste material**

Send:
> "我老婆下周生日，刚搬完家她特别累。我帮你贴几段最近的日记片段："
> [paste 3-5 sentences mentioning her, e.g., "她说办公室那股味道把她快逼疯了", "上周她半夜没睡好，说脑子转个不停", "她最近迷上了手冲咖啡，但还没买器具"]

Expected:
- Greeting (Round 0)
- Round 1 parsing happens silently (no Round 2 needed — info is sufficient)
- Round 3 mirror appears: should reference "刚搬完家" + "她累" specifically
- 3 plans appear with all 5 fields
- One plan likely picks up "办公室那股味道" or "手冲咖啡" as a basis
- Recommendation ranking at the end

If any of the 3 plans fail to cite a concrete detail from the pasted material → bug in psych-framework or generate-3plans. File an issue, fix before continuing.

- [ ] **Step 4: Run scenario B — Crisis**

In a fresh session:
> "和女朋友冷战三天了，因为我忘了她妈手术那天，想道歉"

Expected:
- Round 1 marks `crisis = true`
- Round 3 mirror appears
- Crisis warning appears between mirror and confirmation question
- After user confirms, plans skew small/private/restrained
- Recommendation usually points to Plan 3 (safe)
- Footer reminding "礼物是诚意的载体，不是道歉本身"

- [ ] **Step 5: Run scenario C — No material**

In a fresh session:
> "我女朋友生日快到了，推荐个礼物"

Expected:
- Round 1 marks info as insufficient
- Round 2 asks ONE memory-trigger question
- User answers vaguely ("不知道啊")
- Round 3 mirror still happens, gracefully
- Plans generate WITHOUT inventing details
- Basis fields honestly say "基于你的场景描述，没有从素材里挖到具体细节"

- [ ] **Step 6: Note all bugs found in a checklist; fix iteratively**

Common bugs and where to fix:
- AI-tone message → strengthen `prompts/self-review.md` question 2
- Vague gift field → strengthen `prompts/generate-3plans.md` "Hard requirements"
- Crisis warning missing → check `prompts/round1-parsing.md` keyword list
- Greeting too long / cringe → tune `prompts/greeting.md`

After each fix, commit:
```bash
git add prompts/<file>.md
git commit -m "fix: <specific issue>"
```

- [ ] **Step 7: Re-run scenarios A/B/C after fixes**

Repeat until all three scenarios pass cleanly. THIS IS THE GATE — do not proceed to writing examples (Task 17+) until the skill behaves correctly end-to-end on these three scenarios.

---

### Task 16: Tag a v0.1 internal milestone

- [ ] **Step 1: Tag**

```bash
cd <PROJECT_ROOT>
git tag -a v0.1.0-alpha -m "Internal alpha: SKILL.md + 9 prompts complete, smoke tests pass"
```

- [ ] **Step 2: Verify**

```bash
git tag -l
```

Expected: `v0.1.0-alpha` listed.

---

### Task 17: Write example examples/birthday-busy-wife.md

**Files:**
- Create: `<PROJECT_ROOT>/examples/birthday-busy-wife.md`

The example is a complete user↔assistant transcript demonstrating a successful flow. It serves as: (1) in-context exemplar for the skill, (2) README demo material, (3) regression test reference.

- [ ] **Step 1: Run scenario A from Task 15 one more time, in a clean session**

- [ ] **Step 2: Copy the full transcript into examples/birthday-busy-wife.md**

Format the file as:

```markdown
# Example: 生日 + 疲惫期老婆

**Scenario tags:** birthday | partner | tender | material-paste-mode

## Conversation

### User
我老婆下周生日，刚搬完家她特别累。

我贴几段最近日记给你：
- 9.12 「她说办公室那股味道把她快逼疯了」
- 9.18 「她半夜没睡好，说脑子转个不停」
- 9.20 「她最近开始研究手冲咖啡，但说还没买器具」

### Gift-speaks
[paste actual greeting + round-1 silent + round-3 mirror]

### User
对，就是这样。

### Gift-speaks
[paste actual 3-plan output + recommendation]

---

## Why this example works

- Paste-mode usage with concrete dated entries
- All 3 plans cite the pasted material (脱毛膏／手冲器具／睡眠改善)
- Recommendation points to the plan that addresses *the most recent and emotional* signal
- Talking-point fields can be said out loud without sounding fake
```

- [ ] **Step 3: Commit**

```bash
cd <PROJECT_ROOT>
git add examples/birthday-busy-wife.md
git commit -m "docs: add birthday-busy-wife example transcript"
```

---

### Task 18: Write example examples/apology-cold-war.md

**Files:**
- Create: `<PROJECT_ROOT>/examples/apology-cold-war.md`

- [ ] **Step 1: Run scenario B from Task 15 in a clean session**

- [ ] **Step 2: Capture transcript** — same format as Task 17, save to `examples/apology-cold-war.md`.

The "Why this example works" section must call out:
- Crisis flag detection
- Crisis warning appearing between mirror and confirmation
- All 3 plans being small / private / restrained
- Recommendation pointing to Plan 3
- Footer reminder appearing

- [ ] **Step 3: Commit**

```bash
git add examples/apology-cold-war.md
git commit -m "docs: add apology-cold-war crisis example"
```

---

### Task 19: Write example examples/support-stress.md

**Files:**
- Create: `<PROJECT_ROOT>/examples/support-stress.md`

- [ ] **Step 1: Run a tender/心疼 scenario**

> "她最近考研压力很大，每天复习到凌晨，我心疼她。"

- [ ] **Step 2: Capture transcript**, save to `examples/support-stress.md`.

Must demonstrate:
- Plans that lean toward acts-of-service (e.g., "我把这周饭都包了") rather than objects alone
- At least one plan that's "experience > thing"
- Talking-point that doesn't undermine her effort ("加油" is forbidden; she doesn't need cheerleading)

- [ ] **Step 3: Commit**

```bash
git add examples/support-stress.md
git commit -m "docs: add support-stress tender example"
```

---

### Task 20: Write example examples/no-material-fallback.md

**Files:**
- Create: `<PROJECT_ROOT>/examples/no-material-fallback.md`

- [ ] **Step 1: Run scenario C from Task 15**

- [ ] **Step 2: Capture transcript**, save to `examples/no-material-fallback.md`.

Must demonstrate:
- Honest acknowledgment of weak material
- Basis fields stating "基于你的场景描述" rather than inventing details
- Recommendation still being decisive (certainty principle)
- Quality is reduced but skill doesn't fail or fabricate

- [ ] **Step 3: Commit**

```bash
git add examples/no-material-fallback.md
git commit -m "docs: add no-material fallback example"
```

---

### Task 21: Write example examples/iteration-flow.md

**Files:**
- Create: `<PROJECT_ROOT>/examples/iteration-flow.md`

- [ ] **Step 1: Continue scenario A from Task 17 with iteration commands**

After receiving the 3 plans, send:
> "第二个太贵了，再换一个 ¥300 以内的。"

Then:
> "把第一个的文案改一下，更轻一点，不要太煽情。"

- [ ] **Step 2: Capture transcript**, save to `examples/iteration-flow.md`.

Must demonstrate:
- Single-plan replacement preserving plans 1 and 3
- Budget constraint applied to the replacement
- Message-only rewrite that keeps the gift/timing/basis fields untouched
- Recommendation updated after each iteration

- [ ] **Step 3: Commit**

```bash
git add examples/iteration-flow.md
git commit -m "docs: add iteration-flow example"
```

---

### Task 22: Write README.md (Chinese, main)

**Files:**
- Create: `<PROJECT_ROOT>/README.md`

- [ ] **Step 1: Write the file**

The Chinese README is the project's front door. Structure:

```markdown
# 礼说 / gift-speaks

> 来，让你的礼物为你说话。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skill](https://img.shields.io/badge/Claude-Skill-blue.svg)](https://docs.claude.com/en/docs/build-with-claude/agent-skills)
[Languages: [中文](README.md) | [English](README.en.md) | [日本語](README.ja.md)]

![Demo](demo/demo.gif)

## 这是什么

一个 Claude skill。它帮你给亲密关系里的那个 TA 选一份**真的戳到心里**的礼物——不是热门榜单，不是 AI 套话，而是**基于你笔记里 TA 的私密细节**生成的 3 套「礼物 + 文案 + 怎么送」组合。

**核心信念：礼物的目的是增强人与人之间的信任。**

## 一分钟看懂

![Hero screenshot](demo/hero-screenshot.png)

输入：
> "我老婆下周生日，刚搬完家她特别累。"
> + 你的笔记文件夹（或粘贴几段）

输出（3 套，每套 5 个字段）：
- 🎁 具体礼物（型号 / 价位 / 在哪买 / 多久到）
- ✉️ 送礼时说的话
- 🕯️ 什么时候、用什么方式送
- 🗝️ 这套方案为什么戳——引用你笔记里哪条细节
- 💬 如果 TA 问"你怎么想到的"，你可以这样说

最后：明确的推荐排序，不让你陷入选择困难。

## 谁该用

✅ 用：
- 想给伴侣 / 暧昧对象 / 喜欢的人送礼，但卡壳
- 知道 TA 大概喜欢啥，但不知道怎么落到具体礼物 + 文案
- 关系修复期，想表达诚意但又怕踩雷

❌ 不用：
- 送领导 / 客户 / 同事（场景完全不同，用别的工具）
- 送父母 / 兄弟姐妹（这是另一类情感逻辑）
- 想要"AI 推送爆款礼物排行榜"（这个 skill 不做那件事）

## 怎么用

1. 把这个 repo clone 下来，复制或软链接到 `~/.claude/skills/gift-speaks/`
2. 在 Claude Code 里开始一段新对话
3. 直接说出你的处境（"我老婆下周生日..."）
4. skill 会自动激活，跟你聊几句，给你 3 套方案
5. 不满意？直接说"换一个 / 太贵了 / 改文案"

## 隐私

- 你的笔记只在本地分析，**不会传任何东西到云端**
- skill 默认只读最近 6 个月，可以指定关键词过滤
- 可以排除你不想被读的关键词（比如前任名字）
- 不会原文引用，只引用"提炼过的细节"

## 看例子

- [examples/birthday-busy-wife.md](examples/birthday-busy-wife.md) — 生日 + 老婆累
- [examples/apology-cold-war.md](examples/apology-cold-war.md) — 冷战 + 道歉
- [examples/support-stress.md](examples/support-stress.md) — TA 压力大、我心疼
- [examples/no-material-fallback.md](examples/no-material-fallback.md) — 没素材怎么办
- [examples/iteration-flow.md](examples/iteration-flow.md) — 不满意怎么改

## 设计哲学

- **礼物只是载体，信任才是目的**
- **私密细节 > 任何抽象框架**
- **AI 给方案，但要让用户能"内化"成自己的话**
- **危机场景下，礼物是辅助、不是替代**
- **三轮对话上限，不做问卷调查**

完整设计文档：[docs/superpowers/specs/2026-05-25-gift-speaks-design.md](docs/superpowers/specs/2026-05-25-gift-speaks-design.md)

## 不做什么

- ❌ 礼物电商导购 / 跳转购买链接（链接会过期，给关键词更长久）
- ❌ 跨会话记忆 / 云端账号（隐私第一）
- ❌ 多智能体编排（一个 skill 能搞定，不过度工程）
- ❌ 商务 / 亲情 / 友情场景（专注一件事做好）

## License

MIT — 见 [LICENSE](LICENSE)。

## 贡献

欢迎 issue 和 PR。修改 prompt 时请提供前后对比的对话样本——prompts 不是普通代码，需要看实际效果验证。
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "docs: add Chinese README"
```

---

### Task 23: Write README.en.md (English)

**Files:**
- Create: `<PROJECT_ROOT>/README.en.md`

- [ ] **Step 1: Translate README.md to English**

Keep the same structure and section order. Translate idiomatically — don't word-by-word.

Key adaptations:
- "戳到心里" → "actually lands"
- "AI 套话" → "AI clichés"
- "选择困难" → "decision paralysis"
- "踩雷" → "misfire"

Adapt the example input to:
> "It's my wife's birthday next week, we just moved, she's exhausted."

Keep the language switcher line at top:
> Languages: [中文](README.md) | [English](README.en.md) | [日本語](README.ja.md)

- [ ] **Step 2: Commit**

```bash
git add README.en.md
git commit -m "docs: add English README"
```

---

### Task 24: Write README.ja.md (Japanese)

**Files:**
- Create: `<PROJECT_ROOT>/README.ja.md`

- [ ] **Step 1: Translate to Japanese**

Same structure. Use natural Japanese (敬語 light, not too formal).

- "礼说" Japanese branding: "ギフトに語らせて"
- Example: 「来週は妻の誕生日、引っ越したばかりで疲れきっている」

Keep the language switcher at top.

- [ ] **Step 2: Commit**

```bash
git add README.ja.md
git commit -m "docs: add Japanese README"
```

---

### Task 25: Record demo.gif

**Files:**
- Create: `<PROJECT_ROOT>/demo/demo.gif`

- [ ] **Step 1: Install ScreenToGif (or equivalent)**

Download from https://www.screentogif.com/ — free, Windows-native, captures region + outputs gif directly.

- [ ] **Step 2: Plan the demo script (60-90 seconds)**

Sequence to record:

1. (5s) Show terminal: `claude` starts a fresh session
2. (10s) User types: "我老婆下周生日，刚搬完家她特别累"
3. (5s) Skill activates, greeting appears
4. (10s) User pastes 3 short note snippets
5. (5s) Round 3 mirror appears — emphasize this "I see you" moment
6. (15s) 3 plans appear; scroll showing all 3
7. (10s) Recommendation appears
8. (10s) User says "第二个太贵了，换一个"; new plan 2 appears
9. (5s) End frame: gift-speaks logo / project name

- [ ] **Step 3: Record**

Run the demo in Claude Code. Record at 1280×720, 15 fps. Trim to 60-90 seconds. Export as `.gif`, keep file size < 5 MB (use ScreenToGif's color reduction).

- [ ] **Step 4: Save as `demo/demo.gif`**

- [ ] **Step 5: Commit**

```bash
cd <PROJECT_ROOT>
git add demo/demo.gif
git commit -m "docs: add 60-90s demo gif"
```

---

### Task 26: Capture demo/hero-screenshot.png and demo/wow-moment.png

**Files:**
- Create: `<PROJECT_ROOT>/demo/hero-screenshot.png`
- Create: `<PROJECT_ROOT>/demo/wow-moment.png`

- [ ] **Step 1: Take hero-screenshot.png**

Capture the moment where the input prompt + first plan output are both visible. Use Win+Shift+S (Windows snip tool). Aim for 1200×800.

Crop to show clearly:
- The user's situation message at top
- One full plan with all 5 fields visible

Save as `demo/hero-screenshot.png`.

- [ ] **Step 2: Take wow-moment.png**

Capture the Round 3 mirror — the "I see you" moment where the skill restates the user's situation. This is the screenshot that goes viral on Twitter / Xiaohongshu.

Aim for 1200×600. Frame to show:
- The user's input above
- The mirror response highlighted

Save as `demo/wow-moment.png`.

- [ ] **Step 3: Commit**

```bash
git add demo/hero-screenshot.png demo/wow-moment.png
git commit -m "docs: add hero and wow-moment screenshots"
```

---

### Task 27: Manual QA pass — full verification checklist

This is a sit-down session running every check from the spec's verification section. Don't skip — this is the last gate before publishing.

- [ ] **Step 1: Functional verification**

For each scenario (birthday / apology / support / no-material), run all three material modes (path / paste / none). Check:
- Output uses 5 fields per plan ✅
- 3 plans differ meaningfully (not paraphrased copies) ✅
- No AI-cliché message phrases (e.g., "may this gift bring you...") ✅
- Recommendation ranking is present and specific ✅
- Crisis scenarios show warning ✅

- [ ] **Step 2: Privacy verification**

- Set up a fake notes folder containing one file mentioning "ex-girlfriend Sarah" and a separate file about current partner "Mei". Use path mode.
- Verify skill ASKS for filter keywords (default 6mo window + explicit "must-include" + optional "must-exclude").
- Verify if user says "exclude Sarah", skill doesn't surface anything from Sarah's file.

- [ ] **Step 3: No-material fallback verification**

- Point skill at empty / irrelevant folder.
- Verify basis fields honestly state "no detail found from material" — no fabrication.

- [ ] **Step 4: Iteration verification**

- After initial 3 plans, run each of the 5 iteration patterns from `prompts/iterate.md`.
- Verify each works without re-interrogation.

- [ ] **Step 5: Multilingual verification**

- Repeat scenario A in English. Verify skill replies in English.
- Repeat in Japanese. Verify skill replies in Japanese.
- Check that README.en.md and README.ja.md don't have obvious translation errors (have someone fluent skim if possible).

- [ ] **Step 6: Real user check (golden gate)**

Find 2-3 real people NOT involved in building this — ideally a friend who's struggled with gift-giving recently.

Have them use the skill for a real upcoming gift moment. Observe (not assist):
- Do they say "wow this actually gets it" at any point?
- Would they actually use the recommended plan?
- Does the talking-point feel natural in their mouth?

If 0 out of 3 users say "wow", this is a failure mode that needs prompt tuning. Don't ship.

- [ ] **Step 7: Document any issues found and fix iteratively**

Each fix → commit → re-test the affected scenario.

---

### Task 28: Publish to GitHub

**Files:** none (remote operation)

- [ ] **Step 1: Create GitHub repository**

Run:
```bash
gh repo create gift-speaks --public --description "来，让你的礼物为你说话 / Let your gift speak for you. A Claude skill that helps you choose meaningful gifts + write resonant messages for someone you love, by mining your own notes for private details." --source <PROJECT_ROOT> --remote origin
```

Or via web at https://github.com/new — name `gift-speaks`, public, no README/license/.gitignore (we already have ours).

- [ ] **Step 2: Push**

```bash
cd <PROJECT_ROOT>
git push -u origin main
git push --tags
```

Expected: repo populated, `v0.1.0-alpha` tag visible.

- [ ] **Step 3: Verify the repo renders correctly**

Open the GitHub repo page in browser. Check:
- README.md renders Chinese correctly (no `???` characters)
- demo.gif autoplays in the README
- Language switcher links work
- LICENSE displayed in the right sidebar

- [ ] **Step 4: Create v0.1.0 release**

```bash
gh release create v0.1.0 --title "v0.1.0 — first public release" --notes "First public release of gift-speaks. 9 prompts, 5 examples, trilingual README, demo gif. See README for usage."
git tag v0.1.0
git push --tags
```

- [ ] **Step 5: Announce (optional, when you're ready)**

Suggested first-touch channels:
- Twitter / X: post hero-screenshot + repo link + a one-line hook ("I built a Claude skill that reads your notes and helps you give better gifts to the people you love")
- Xiaohongshu: post demo.gif + "礼说" hashtag, target couples/young professionals
- Hacker News: skip until you have stronger evals (HN destroys half-baked projects)

---

## Self-review (executed at plan-write time)

**Spec coverage:**
- ✅ North Star — Task 4 (SKILL.md), Task 22 (README)
- ✅ Naming / branding — Tasks 4 / 22-24
- ✅ Three-round conversation — Tasks 7 / 8 / 9
- ✅ Material three-modes + privacy filters — Task 8
- ✅ No-material fallback — Tasks 12 / 20
- ✅ Priority function (5 factors) — Task 11
- ✅ Psychology framework (love language / attachment / MBTI) — Task 11
- ✅ 5-field output — Task 12
- ✅ Certainty principle (ranking + purchasability) — Task 12
- ✅ Crisis warning — Tasks 10 / 18
- ✅ Self-review loop — Task 13
- ✅ Iteration round 4+ — Tasks 14 / 21
- ✅ Demo gif + screenshots — Tasks 25-26
- ✅ Trilingual README — Tasks 22-24
- ✅ Verification scenarios — Task 27

**Placeholder scan:** All steps contain concrete content. Where content is generated at runtime (transcripts, gifs, screenshots), the step describes exactly what to capture and how.

**Type consistency:** Field names used consistently across SKILL.md, generate-3plans.md, self-review.md, iterate.md, examples (🎁 礼物 / ✉️ 文案 / 🕯️ 时机 & 呈现 / 🗝️ 依据 / 💬 话术). Crisis-flag name `crisis = true` consistent across round1-parsing.md, round3-locate.md, crisis-warning.md, generate-3plans.md.

**Scope:** Single MVP scope. Stretches deferred (e.g., Hacker News announcement, eval suite).
