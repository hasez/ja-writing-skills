# ja-writing-skills

日本語ドキュメントを書く・直すための Claude Code Skill をまとめたリポジトリ。
プロジェクトへコピーして使う個人用のテンプレートである。

## 収録する Skill

| Skill                   | 役割                                                           | 出典                  | ライセンス       |
| ----------------------- | -------------------------------------------------------------- | --------------------- | ---------------- |
| `japanese-tech-writing` | 日本語技術文書の文章規範（整形、論理の組み立て、冗長さの除去） | k16shikano gist       | 明示なし（後述） |
| `natural-japanese`      | AI っぽさと読みにくさを機械で検査する仕上げ                    | coji/natural-japanese | MIT              |

## 使い方

### 1. プロジェクトへ Skill をコピーする

対象プロジェクトの `.claude/skills/` に、この 2 ディレクトリを置く。

```bash
cp -R skills/japanese-tech-writing  <project>/.claude/skills/
cp -R skills/natural-japanese        <project>/.claude/skills/
```

### 2. `CLAUDE.md` にルールを追記する

`CLAUDE.snippet.md` の中身を、対象プロジェクトの `CLAUDE.md` へ貼り付ける。
このルールにより、japanese-tech-writing は文書を書くたびに必ず効き、natural-japanese は明示指示のときだけ動く。

### 3. 動作確認（任意）

natural-japanese の機械チェックは `uv` があれば動く。

```bash
cd <project>/.claude/skills/natural-japanese
uv run scripts/lint.py --json --genre tech <対象ファイル>
```

## 2 つの Skill の関係

役割は分かれている。
natural-japanese は整形を自分の担当としない。
整形のほうは japanese-tech-writing に任せる。
このため両者は重ならず、必須（japanese-tech-writing）と任意（natural-japanese）として併存できる。

## ライセンスと出典

- `natural-japanese` は MIT である。`skills/natural-japanese/LICENSE` に原文を同梱する。出典は https://github.com/coji/natural-japanese
- `japanese-tech-writing` は出典 gist に明示ライセンスがない。https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d を参照して取り込んだ。ライセンスが不明なため、本リポジトリは**非公開**で管理する。公開や再配布をする前に、作者への確認かライセンスの明示を行うこと。

詳細な帰属は `NOTICE.md` に記す。
