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
