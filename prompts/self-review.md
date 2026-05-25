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
