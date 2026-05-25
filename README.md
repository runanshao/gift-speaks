# 礼说 / gift-speaks

> 来，让你的礼物为你说话。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skill](https://img.shields.io/badge/Claude-Skill-blue.svg)](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)

Languages: [中文](README.md) | [English](README.en.md) | [日本語](README.ja.md)

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

1. 把这个 repo clone 下来：
   ```bash
   git clone https://github.com/runan/gift-speaks.git
   ```

2. 复制到 Claude 的 skills 目录：
   ```bash
   # macOS / Linux
   cp -r gift-speaks ~/.claude/skills/gift-speaks

   # Windows（PowerShell）
   Copy-Item -Recurse gift-speaks "$env:USERPROFILE\.claude\skills\gift-speaks"
   ```

3. 在 Claude Code 里开始一段新对话

4. 直接说出你的处境：
   > "我老婆下周生日，刚搬完家她特别累..."

5. skill 会自动激活，跟你聊几句，给你 3 套方案

6. 不满意？直接说"换一个 / 太贵了 / 改文案"，无限迭代

## 隐私

- 你的笔记只在本地分析，**不会传任何东西到云端**
- skill 默认只读最近 6 个月，可以指定关键词过滤
- 可以排除你不想被读的关键词（比如前任名字）
- 不会原文引用，只引用"提炼过的细节"

## 看例子

- [examples/birthday-busy-wife.md](examples/birthday-busy-wife.md) — 生日 + 老婆累
- [examples/apology-cold-war.md](examples/apology-cold-war.md) — 冷战 + 道歉（含危机场景提醒）
- [examples/support-stress.md](examples/support-stress.md) — TA 压力大、我心疼
- [examples/no-material-fallback.md](examples/no-material-fallback.md) — 没素材怎么办
- [examples/iteration-flow.md](examples/iteration-flow.md) — 不满意怎么改

## 设计哲学

- **礼物只是载体，信任才是目的**
- **私密细节 > 任何抽象框架**（Gary Chapman 5 种爱的语言等心理学骨架，但具体细节永远压倒抽象框架）
- **AI 给方案，但要让用户能"内化"成自己的话**
- **危机场景下，礼物是辅助、不是替代**（冷战/道歉时会给出反提醒）
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

欢迎 issue 和 PR。修改 prompt 时请提供前后对比的对话样本——prompts 不是普通代码，需要看实际效果才能验证改动是否有效。
