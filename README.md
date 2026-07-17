# ja-writing-skills

日本語ドキュメントを書く・直すための Claude Code Skill をまとめたリポジトリ。
プロジェクトへコピーして使う個人用のテンプレートである。

## 収録する Skill

| Skill                   | 役割                                                           | 出典                  | ライセンス       |
| ----------------------- | -------------------------------------------------------------- | --------------------- | ---------------- |
| `japanese-tech-writing` | 日本語技術文書の文章規範（整形、論理の組み立て、冗長さの除去） | k16shikano gist       | Unlicense        |
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

## 運用と更新（同期）

このリポジトリを正本とし、各プロジェクトへは取り込み方に応じて更新を反映する。

### 方式 A：コピーで取り込む（共有リポジトリ向け・推奨）

会社の共有リポジトリ（GitLab など）には、Skill を **verbatim コピー**で置く。
submodule は使わない。
チームメンバーがこの private リポジトリにアクセスできず、`git submodule update` が失敗するためである。

更新の反映は、正本を pull してからコピーし直す。

```bash
# このリポジトリ側で最新を取得
git -C <path-to>/ja-writing-skills pull

# 対象プロジェクトへ上書きコピー
rm -rf <project>/.claude/skills/japanese-tech-writing <project>/.claude/skills/natural-japanese
cp -R <path-to>/ja-writing-skills/skills/japanese-tech-writing <project>/.claude/skills/
cp -R <path-to>/ja-writing-skills/skills/natural-japanese        <project>/.claude/skills/
```

### 方式 B：submodule で参照する（自分の個人プロジェクト向け）

アクセス権が自分だけに閉じる個人プロジェクトでは、submodule で参照してよい。

```bash
cd <your-project>
git submodule add git@hasez.github.com:hasez/ja-writing-skills.git .ext/ja-writing-skills
ln -s ../../.ext/ja-writing-skills/skills/japanese-tech-writing .claude/skills/japanese-tech-writing
ln -s ../../.ext/ja-writing-skills/skills/natural-japanese        .claude/skills/natural-japanese

# 更新の取り込み
git submodule update --remote .ext/ja-writing-skills
```

### 改善を正本へ戻す

各プロジェクトで Skill を直したら、その差分をこのリポジトリへ反映してから push する。
正本を一つに保ち、プロジェクトごとに枝分かれさせない。

## ライセンスと出典

- `natural-japanese` は MIT である。`skills/natural-japanese/LICENSE` に原文を同梱する。出典は https://github.com/coji/natural-japanese
- `japanese-tech-writing` は **Unlicense**（パブリックドメイン相当）である。作者が gist のコメントで明示している（https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d の comment 6210840）。`skills/japanese-tech-writing/UNLICENSE` に原文を同梱する。再配布・改変・公開は自由に行える。

詳細な帰属は `NOTICE.md` に記す。
