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
