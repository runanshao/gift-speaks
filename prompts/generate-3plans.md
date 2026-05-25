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
