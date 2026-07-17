<!--
このファイルの本文ブロックを、対象プロジェクトの CLAUDE.md へそのまま貼り付ける。
パスは配置先に合わせて調整すること（既定は .claude/skills/）。
-->

## Documentation Writing (日本語文書執筆ルール)

日本語ドキュメント（README・設計書・手順書・解説記事・障害報告など）を**新規執筆・推敲・リライト**するときは、次の分担で 2 つの Skill を使う。

1. **必須: `japanese-tech-writing`（`.claude/skills/japanese-tech-writing/`）**
   - 日本語文書を書く/直すたびに**必ず**この規範を適用する。整形（一文一行・脚注・ダッシュ/中黒の扱い）、段落と論証の構成、論理の厳密さ、冗長・LLM 口調の排除を担う。
   - 出典: k16shikano gist（https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d ）
2. **任意: `natural-japanese`（`.claude/skills/natural-japanese/`）**
   - AI 臭・単調さ・読みにくさの機械 lint による仕上げ検査。**ユーザーが明示的に求めたときだけ**（`/natural-japanese` の実行、または「AI 臭を消して」「自然にして」「lint して」等の指示）使う。文書執筆で自動起動させない。
   - `uv run scripts/lint.py` が使える（devcontainer に `uv` あり）。出典: coji/natural-japanese（MIT, https://github.com/coji/natural-japanese ）

> 2 つは設計上補完関係（natural-japanese は整形を対象外とし japanese-tech-writing に委ねる）。役割が重複しないので併存させてよい。
