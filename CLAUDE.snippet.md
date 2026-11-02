<!--
このファイルの本文ブロックを、対象プロジェクトの CLAUDE.md へそのまま貼り付ける。
パスは配置先に合わせて調整すること（既定は .claude/skills/）。
-->

## Documentation Writing (日本語文書執筆ルール)

日本語ドキュメント（README・設計書・手順書・解説記事・障害報告など）を**新規執筆・推敲・リライト**するときは、次の分担で 2 つの Skill を使う。

1. **必須: `japanese-tech-writing`（`.claude/skills/japanese-tech-writing/`）**
   - 日本語文書を書く/直すたびに**必ず**この規範を適用する。この Skill の description は技術書・記事・解説文向けなので、設計書・手順書・障害報告などへは、この CLAUDE.md の指示で明示的に読み込ませる。段落と論証の構成、論理の厳密さ、視点と語り、演出の抑制、LLM 口調と翻訳調の比喩の排除、冗長の排除を担う。
   - 出典: k16shikano gist（https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d ）
2. **任意: `natural-japanese`（`.claude/skills/natural-japanese/`）**
   - AI 臭・単調さ・読みにくさの機械 lint による仕上げ検査（議事録・レポート・メモ・ガイド・スライドの文書種別ガイドを含む）。**ユーザーが明示的に求めたときだけ**（`/natural-japanese` の実行、または「AI 臭を消して」「自然にして」「lint して」等の指示）使う。文書執筆で自動起動させない。
   - `uv run scripts/lint.py` で検査できる（`uv` が必要）。出典: coji/natural-japanese（MIT, https://github.com/coji/natural-japanese ）

> 2 つは補完関係で、japanese-tech-writing は技術文書の文章規範、natural-japanese は機械 lint による仕上げを担う。併存させてよい。
