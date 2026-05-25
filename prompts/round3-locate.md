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
