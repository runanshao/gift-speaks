# 礼说 / gift-speaks（ギフトに語らせて）

> あなたの想いを、ギフトに語らせて。

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skill](https://img.shields.io/badge/Claude-Skill-blue.svg)](https://docs.anthropic.com/en/docs/build-with-claude/tool-use)

Languages: [中文](README.md) | [English](README.en.md) | [日本語](README.ja.md)

## これは何？

Claude スキルです。パートナー・恋人・気になる人への**本当に刺さる**プレゼントを選ぶのを手伝います。トレンドランキングでも、AI の決まり文句でもなく、**あなたが相手について知っていることと、いまの状況**をもとに「プレゼント＋メッセージ＋渡し方」3 セットを提案します。ノートがあれば使えます。なければ少し話すだけで大丈夫です。

**コアの信念：プレゼントの目的は、ふたりの間の信頼を深めること。**

## 30 秒でわかる

![Hero screenshot](demo/hero-screenshot.png)

入力：
> 「来週は妻の誕生日。引っ越したばかりで彼女は疲れきっている。」
> その後、少し会話するだけ——メモがあれば貼り付けてもいいし、何もなくても大丈夫

出力（3 プラン、それぞれ 5 フィールド）：
- 🎁 具体的なギフト（型番 / 価格 / 購入先 / 配送日数）
- ✉️ 渡すときに言う言葉
- 🕯️ いつ・どうやって渡すか
- 🗝️ なぜこのプランが刺さるのか——あなたのノートのどのエピソードを使ったか
- 💬 「なんでこれにしたの？」と聞かれたときに言える一言

そして：決断麻痺に陥らないための、明確な推薦順位。

## こんな人に

✅ 使う：
- パートナー・恋人・気になる人へのプレゼントに悩んでいる
- 何が好きかはわかるけど、具体的なギフト＋メッセージに落とせない
- 関係修復中で、誠意を伝えたいけど地雷を踏みたくない

❌ 使わない：
- 上司・クライアント・同僚へのギフト（まったく異なる感情ロジック）
- 両親・兄弟へのギフト（異なる関係性）
-「AI による人気ギフトランキング」が欲しい（このスキルはそれをしません）

## 使い方

1. リポジトリをクローン：
   ```bash
   git clone https://github.com/runanshao/gift-speaks.git
   ```

2. Claude のスキルディレクトリにコピー：
   ```bash
   # macOS / Linux
   cp -r gift-speaks ~/.claude/skills/gift-speaks

   # Windows（PowerShell）
   Copy-Item -Recurse gift-speaks "$env:USERPROFILE\.claude\skills\gift-speaks"
   ```

3. Claude Code で新しい会話を開始

4. 状況を話しかけるだけ：
   > 「来週は妻の誕生日、引っ越したばかりで疲れきっている...」

5. スキルが自動的に起動し、少し話を聞いてから 3 プランを提案

6. 気に入らなければ「2 番目を変えて / 高すぎる / メッセージ書き直して」と言うだけ。何度でも調整可能。

## プライバシー

- ノートはすべてローカルで分析され、**クラウドには何もアップロードされません**
- デフォルトは直近 6 ヶ月のみ読み込み。キーワードフィルタも設定可能。
- 読み込みから除外したいキーワード（元恋人の名前など）を指定できます
- 原文の引用はしません——「要約した細かいエピソード」のみを参照します

## 使用例

- [examples/birthday-busy-wife.md](examples/birthday-busy-wife.md) — 誕生日 ＋ 疲れた妻
- [examples/apology-cold-war.md](examples/apology-cold-war.md) — 冷戦 ＋ 謝罪（クライシスリマインダーあり）
- [examples/support-stress.md](examples/support-stress.md) — 受験プレッシャーの彼女を支えたい
- [examples/no-material-fallback.md](examples/no-material-fallback.md) — ノートがない場合は？
- [examples/iteration-flow.md](examples/iteration-flow.md) — 気に入らなければどう調整するか

## 設計思想

- **ギフトはメディア、目的は信頼**
- **プライベートな細かい事実 > あらゆる抽象的フレームワーク**（Gary Chapman の 5 つの愛の言語などは骨格として使うが、具体的な事実が常に理論に勝る）
- **AI がプランを出す、でも言葉はあなたのもの**（トーキングポイントはあなた自身の言葉として言えるよう書かれています）
- **クライシスシナリオでは、ギフトは補佐——代替品ではない**（冷戦・謝罪の場面では明示的な注意書きが入ります）
- **会話は最大 3 ラウンド、アンケートにはしない**

詳細な設計ドキュメント：[docs/superpowers/specs/2026-05-25-gift-speaks-design.md](docs/superpowers/specs/2026-05-25-gift-speaks-design.md)

## やらないこと

- ❌ ショッピングリンク（URL は古くなる——検索キーワードの方が長持ちする）
- ❌ セッションをまたぐ記憶 / クラウドアカウント（プライバシー最優先）
- ❌ マルチエージェント構成（1 つのスキルで十分、過剰設計しない）
- ❌ ビジネス / 家族 / 友人向けギフト（1 つのことに集中する）

## ライセンス

MIT — [LICENSE](LICENSE) 参照。

## コントリビューション

Issue や PR 歓迎。プロンプトを変更する場合は、変更前後の会話サンプルを添えてください——プロンプトは通常のコードではなく、実際の動作変化で検証する必要があります。
