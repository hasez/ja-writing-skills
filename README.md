# ja-writing-skills

日本語ドキュメントを書く・直すための Claude Code Skill をまとめたリポジトリ。
プロジェクトへコピーして使う個人用のテンプレートである。

## 収録する Skill

| Skill                   | 役割                                                           | 出典                  | ライセンス       |
| ----------------------- | -------------------------------------------------------------- | --------------------- | ---------------- |
| `japanese-tech-writing` | 日本語技術文書の文章規範（論理の組み立て、LLM 口調と冗長さの除去） | k16shikano gist       | Unlicense        |
| `natural-japanese`      | AI っぽさと読みにくさを機械で検査する仕上げ                    | coji/natural-japanese | MIT              |

## 使い方

### 1. プロジェクトへ Skill をコピーする

対象プロジェクトの `.claude/skills/` に、この 2 ディレクトリを置く。
コマンドはこのリポジトリの直下で実行する。

```bash
mkdir -p <project>/.claude/skills
cp -R skills/japanese-tech-writing  <project>/.claude/skills/
cp -R skills/natural-japanese        <project>/.claude/skills/
```

### 2. `CLAUDE.md` にルールを追記する

`CLAUDE.snippet.md` の中身を、対象プロジェクトの `CLAUDE.md` へ貼り付ける。
このルールにより、japanese-tech-writing は文書を書くたびに必ず効き、natural-japanese は明示指示のときだけ動く。
japanese-tech-writing の description は技術書・記事・解説文向けなので、設計書や手順書への適用は自動起動ではなく、CLAUDE.md の指示に依存する。

### 3. 動作確認（任意）

natural-japanese の機械チェックには `uv` が必要である。

```bash
cd <project>/.claude/skills/natural-japanese
uv run scripts/lint.py --json --genre tech <対象ファイル>
```

## 2 つの Skill の関係

japanese-tech-writing は書くときに従う文章規範を、natural-japanese は機械 lint による仕上げの検査と文書種別ガイド（議事録・レポート・メモ・ガイド・スライド）を担う。
LLM 口調や翻訳調は両方が扱うので、検査の対象は一部重なる。
補完関係にあり、必須（japanese-tech-writing）と任意（natural-japanese）として併存できる。

## 運用と更新（同期）

このリポジトリを正本とし、各プロジェクトへは取り込み方に応じて更新を反映する。

### 上流からこのリポジトリへ取り込む

各 Skill は上流の verbatim コピーである。
更新するときは上流を clone し、差分を確認してから上書きする。

```bash
git clone --depth 1 https://github.com/coji/natural-japanese.git <tmp>/coji
git clone https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d.git <tmp>/gist

diff -r <tmp>/coji/skills/natural-japanese skills/natural-japanese
diff <tmp>/gist/SKILL.md skills/japanese-tech-writing/SKILL.md
```

差分を確認したら上書きし、`NOTICE.md` の「同期済みの版」を更新する。
natural-japanese の `LICENSE` は上流の `skills/` 配下にないので、上書きで消さない。

### 方式 A：コピーで取り込む（共有リポジトリ向け・推奨）

会社の共有リポジトリ（GitLab など）には、Skill を **verbatim コピー**で置く。
submodule は使わない。
チームメンバーがこの private リポジトリにアクセスできず、`git submodule update` が失敗するためである。

更新の反映は、正本を pull してからコピーし直す。
上書きはプロジェクト側の改善を消すので、先に `diff -r` で差分を確認し、必要なら正本へ戻す（下記）。

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
# git@hasez.github.com は筆者の SSH ホスト別名。環境に合わせて読み替える
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
ただし上流の verbatim コピーを保つ方針なので、上流へ送るべき改善（upstream への PR）と、このリポジトリ独自の変更は区別し、独自の変更は `NOTICE.md` に記す。

## ライセンスと出典

- `natural-japanese` は MIT である。`skills/natural-japanese/LICENSE` に原文を同梱する。出典は https://github.com/coji/natural-japanese
- `japanese-tech-writing` は **Unlicense**（パブリックドメイン相当）である。作者が gist のコメントで明示している（https://gist.github.com/k16shikano/fd287c3133457c4fd8f5601d34aa817d の comment 6210840）。`skills/japanese-tech-writing/UNLICENSE` に原文を同梱する。再配布・改変・公開は自由に行える。

詳細な帰属は `NOTICE.md` に記す。
