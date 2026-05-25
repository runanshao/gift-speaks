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
