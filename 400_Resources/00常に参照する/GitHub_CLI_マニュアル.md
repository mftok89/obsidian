# GitHub CLI (`gh`) 導入・運用マニュアル

GitHub CLI（コマンド名 `gh`）は、GitHub が公式に提供するコマンドラインツールです。
ブラウザを開かずに、ターミナルから Issue・プルリクエスト（PR）・リポジトリ・リリースなどを操作できます。

> このマニュアルは 2026 年時点の `gh` v2.88 系を基準に記載しています。最新版・最新の手順は公式ページ <https://cli.github.com> を参照してください。

---

## 目次

1. [GitHub CLI とは](#1-github-cli-とは)
2. [インストール方法](#2-インストール方法)
   - [2.1 Windows](#21-windows)
   - [2.2 macOS](#22-macos)
   - [2.3 Linux](#23-linux)
   - [2.4 WSL（Windows Subsystem for Linux）](#24-wslwindows-subsystem-for-linux)
   - [2.5 インストールの確認](#25-インストールの確認)
3. [初期設定（ログイン・認証）](#3-初期設定ログイン・認証)
4. [日々の使い方](#4-日々の使い方)
   - [4.1 リポジトリ操作](#41-リポジトリ操作)
   - [4.2 Issue 操作](#42-issue-操作)
   - [4.3 プルリクエスト操作](#43-プルリクエスト操作)
   - [4.4 ブランチとよくある開発フロー](#44-ブランチとよくある開発フロー)
   - [4.5 リリース操作](#45-リリース操作)
   - [4.6 GitHub Actions の確認](#46-github-actions-の確認)
   - [4.7 Gist（コード断片の共有）](#47-gistコード断片の共有)
5. [便利な使い方・Tips](#5-便利な使い方・tips)
6. [トラブルシューティング](#6-トラブルシューティング)
7. [チートシート（よく使うコマンド一覧）](#7-チートシートよく使うコマンド一覧)

---

## 1. GitHub CLI とは

`gh` を使うと、次のような作業をターミナル内で完結できます。

- リポジトリのクローン・作成・閲覧
- Issue / PR の作成・一覧・コメント・クローズ
- PR のレビュー・マージ
- リリースの作成、GitHub Actions の実行状況確認
- 認証情報（トークン）の管理

Git の `git` コマンドが「ローカルのバージョン管理」を担うのに対し、`gh` は「GitHub というサービスそのもの」を操作します。両者は併用するのが基本です。

---

## 2. インストール方法

### 2.1 Windows

いずれか 1 つの方法を選びます。**WinGet が最も手軽**でおすすめです。

**WinGet（Windows 10/11 標準のパッケージ管理）**

```powershell
winget install --id GitHub.cli
```

**Scoop**

```powershell
scoop install gh
```

**Chocolatey**

```powershell
choco install gh
```

**手動インストール**

パッケージ管理を使わない場合は、[リリースページ](https://github.com/cli/cli/releases) から `.msi` インストーラーをダウンロードして実行します。

> インストール後はターミナル（PowerShell / コマンドプロンプト）を**開き直して**ください。`gh` がパスに反映されます。

### 2.2 macOS

**Homebrew（推奨）**

```bash
brew install gh
```

**MacPorts**

```bash
sudo port install gh
```

### 2.3 Linux

**Debian / Ubuntu（公式 APT リポジトリ）**

最新版を使うには、GitHub 公式リポジトリを登録します。

```bash
# 1行ごとに実行する
sudo mkdir -p -m 755 /etc/apt/keyrings

wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg \
  | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null
  
sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
  | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
  
sudo apt update

sudo apt install gh
```

> ディストリビューション同梱版で十分なら `sudo apt install gh` だけでも導入できます（バージョンはやや古い場合があります）。

**Fedora / RHEL 系**

```bash
sudo dnf install gh
```

**Arch Linux**

```bash
sudo pacman -S github-cli
```

### 2.4 WSL（Windows Subsystem for Linux）

Windows 上の Linux 環境（WSL）で `gh` を使う場合は、まず WSL 自体を導入し、その中（Ubuntu など）に `gh` をインストールします。

**手順 1: WSL をインストール**

管理者権限の PowerShell またはコマンドプロンプトを開き、次を実行します。これだけで WSL 本体と既定の Ubuntu がまとめて入ります。

```powershell
wsl --install
```

実行後に **PC を再起動**します。再起動後に Ubuntu が自動起動し、Linux 用のユーザー名とパスワードの作成を求められるので設定します。

```powershell
# 別のディストリビューションを選びたい場合は一覧を確認
wsl --list --online

# 例: ディストリビューションを指定してインストール
wsl --install -d Ubuntu-24.04

# 既にWSLを使っている場合は最新へ更新
wsl --update
```

> Windows 10 (2004 以降) / Windows 11 で利用できます。`wsl --install` が使えない古い環境では、Windows の機能から「Linux 用 Windows サブシステム」を有効化してください。

**手順 2: WSL（Ubuntu）内で `gh` をインストール**

WSL のターミナル（Ubuntu）を開き、Linux と同じく公式 APT リポジトリから導入します。

```bash
# 1行ずつ実行する
sudo mkdir -p -m 755 /etc/apt/keyrings

wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg \
  | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null
  
sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
  | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
  
sudo apt update

sudo apt install gh
```

> 簡易に済ませたい場合は `sudo apt install gh` だけでも導入できます（バージョンはやや古い場合があります）。

**手順 3: 認証**

WSL 内で `gh auth login` を実行します。ブラウザ認証を選ぶと Windows 側のブラウザが開くので、表示されたコードを入力して承認します（[3. 初期設定（ログイン・認証）](#3-初期設定ログイン認証)参照）。

> **注意:** Windows 側にインストールした `gh` と、WSL 側の `gh` は別物です。認証もそれぞれ個別に必要になります。普段 WSL 内で開発するなら WSL 側に入れておくのがおすすめです。

### 2.5 インストールの確認

```bash
gh --version
```

`gh version 2.xx.x ...` のように表示されれば成功です。

**アップデート**

- Windows: `winget upgrade --id GitHub.cli`
- macOS: `brew upgrade gh`
- Linux(APT): `sudo apt update && sudo apt upgrade gh`

---

## 3. 初期設定（ログイン・認証）

インストール後、最初に GitHub アカウントへログインします。

```bash
gh auth login
```

実行すると対話形式で次を聞かれます。

1. **接続先** … `GitHub.com`（通常）か `GitHub Enterprise Server`
2. **プロトコル** … `HTTPS`（推奨）か `SSH`
3. **認証方法** … `Login with a web browser`（推奨）か `Paste an authentication token`

ブラウザ認証を選ぶと 8 桁のワンタイムコードが表示されるので、開いたブラウザに入力して承認します。

**認証状態の確認**

```bash
gh auth status
```

**ログアウト**

```bash
gh auth logout
```

> `gh` でログインすると、`git push` / `git pull` の HTTPS 認証も `gh` が肩代わりしてくれるため、別途トークンを設定する手間が省けます。

---

## 4. 日々の使い方

> **【初心者向け】この章の読み方**
> コマンド例の `#` より後はコメント（説明文）です。実際には入力しなくてかまいません。
> `<run-id>` のように `<>` で囲まれた部分は、実際の値に置き換えて使います。
> コマンドが複数並んでいる場合は、用途に合うものを 1 つ選んで実行してください。

---

### 4.1 リポジトリ操作

> **【補足】リポジトリとは？**
> リポジトリ（repository）とは、コードやファイルの「保管庫」のことです。GitHub 上にあるプロジェクトのひとつひとつがリポジトリです。
> `owner/repo` という形式は「誰のどのプロジェクトか」を示します。例えば `cli/cli` は「cli というユーザー（組織）の cli というリポジトリ」を意味します。

```bash
# リポジトリをクローン（owner/repo 形式でOK）
# → GitHub 上のリポジトリをローカル（自分のPC）にコピーしてくる
gh repo clone mftok89/obsidian

# 新しいリポジトリを作成（対話形式）
# → 質問に答えながら GitHub 上に新しいプロジェクトを作る
# projectsフォルダでcursorを開き、ターミナルから、下記コマンドを入力する
# 「MIT License」を選択すると無難
gh repo create

# 現在のリポジトリをブラウザで開く
# → 今いるフォルダのリポジトリを GitHub のウェブページで確認する
gh repo view --web

# リポジトリ情報を表示
# → 指定したリポジトリの説明・言語・スター数などをターミナルで確認する
gh repo view mftok89/obsidian

# 自分のリポジトリ一覧
# → GitHub に自分が持っているリポジトリを一覧表示する
gh repo list
```

---

### 4.2 Issue 操作

> **【補足】Issue とは？**
> Issue（イシュー）は、バグ報告・機能要望・タスクなどを記録する「チケット」です。
> GitHub の Issues タブに相当する機能を、ターミナルから操作できます。
> Issue には番号（`#123` のような形式）が自動で振られ、コマンドでその番号を指定して操作します。

```bash
# Issue 一覧
# → 現在のリポジトリのオープン中の Issue を一覧表示する
gh issue list

# 自分にアサインされた Issue のみ
# → 自分が担当者に設定されている Issue だけ絞り込んで表示する
gh issue list --assignee "@me"

# Issue を作成（対話形式。--title / --body で直接指定も可）
# → 質問に答えながら新しい Issue を作る
gh issue create

# Issue の詳細を表示
# → 番号 123 の Issue の内容・コメント・ラベルなどをターミナルで確認する
gh issue view 123

# ブラウザで開く
# → 番号 123 の Issue を GitHub のウェブページで開く
gh issue view 123 --web

# コメントを追加
# → 番号 123 の Issue に「対応しました」というコメントを投稿する
gh issue comment 123 --body "対応しました"

# クローズ / 再オープン
# → Issue を「完了（クローズ）」にする / 「未完了（オープン）」に戻す
gh issue close 123
gh issue reopen 123
```

---

### 4.3 プルリクエスト操作

> **【補足】プルリクエスト（PR）とは？**
> プルリクエスト（Pull Request / PR）は、「自分のブランチの変更をメインのコードに取り込んでほしい」と申請する仕組みです。
> チームでのコードレビューはこの PR を通じて行われます。PR にも番号が振られます（例: `#45`）。
>
> **【補足】マージとは？**
> マージ（merge）とは、ブランチで行った変更を本流（main ブランチなど）に統合することです。
> `--squash` オプションをつけると、複数のコミットを 1 つにまとめてからマージします（履歴がすっきりします）。

```bash
# 現在のブランチから PR を作成
# → タイトルと概要を直接指定して PR を作る
gh pr create --title "機能Aを追加" --body "概要..."

# 対話形式で作成
# → 質問に答えながら PR を作る（慣れないうちはこちらがおすすめ）
gh pr create

# PR 一覧
# → 現在のリポジトリのオープン中の PR を一覧表示する
gh pr list

# PR の詳細・差分を確認
# → 番号 45 の PR の概要や変更されたコードを確認する
gh pr view 45
gh pr diff 45

# PR をローカルにチェックアウト（レビュー用）
# → 番号 45 の PR のブランチをローカルに取得して動作確認できるようにする
gh pr checkout 45

# CI（チェック）の状況を確認
# → 番号 45 の PR に対して自動テスト（CI）がパスしているか確認する
gh pr checks 45

# レビューを送る
# → 番号 45 の PR を承認する / 修正をリクエストする
gh pr review 45 --approve
gh pr review 45 --request-changes --body "ここを修正してください"

# マージ（squash マージしてブランチ削除）
# → 番号 45 の PR をマージし、作業ブランチを削除する
gh pr merge 45 --squash --delete-branch
```

---

### 4.4 ブランチとよくある開発フロー

> **【補足】ブランチとは？**
> ブランチ（branch）は「作業の分岐」です。メインのコードを壊さずに新機能の開発や修正を進めるために、専用の作業ラインを作ります。
> 作業が完了したら PR を通じてメインに統合（マージ）します。
>
> **【補足】このフローの全体像**
> `git` コマンド（ローカル操作）と `gh` コマンド（GitHub 操作）を組み合わせて使います。
> 開発の流れは「ブランチ作成 → コード編集 → コミット → プッシュ → PR作成 → レビュー → マージ」です。

1 つの機能追加を `gh` で進める典型的な流れです。

```bash
# 1. 作業ブランチを作成して移動（git コマンド）
# → 「feature/login」という名前の新しいブランチを作り、そこに移動する
git switch -c feature/login

# 2. コードを編集してコミット
# → 変更したファイルをステージングし、コミットメッセージを付けて保存する
git add .
git commit -m "ログイン機能を追加"

# 3. リモートへ push
# → ローカルのブランチを GitHub（リモート）にアップロードする
# → `-u origin feature/login` は「このブランチの push 先を GitHub の同名ブランチに設定する」という意味（初回のみ必要）
git push -u origin feature/login

# 4. PR を作成
# → コミットメッセージから PR のタイトルと本文を自動で生成して作成する
gh pr create --fill          # コミット内容からタイトル・本文を自動入力

# 5. CI とレビューを確認
# → 自動テストの合否を確認する
gh pr checks
# → ブラウザで PR のページを開いてレビューコメントを確認する
gh pr view --web

# 6. 承認後にマージ
# → PR をマージして作業ブランチを削除する
gh pr merge --squash --delete-branch
```

> `--fill` はコミットメッセージから PR タイトル・本文を自動生成する便利オプションです。

---

### 4.5 リリース操作

> **【補足】リリースとは？**
> リリース（release）とは、バージョン番号（例: `v1.0.0`）を付けてソフトウェアの配布版を公開する仕組みです。
> GitHub の「Releases」ページに表示されるものを、コマンドで作成・管理できます。
> バージョン番号は `v1.0.0` のように `v` から始める慣習が一般的です（Semantic Versioning）。

```bash
# リリース一覧
# → これまで作成したリリースの一覧を表示する
gh release list

# リリースを作成（タグ v1.0.0 を付与）
# → バージョン v1.0.0 のリリースを作成し、リリースノートを添付する
gh release create v1.0.0 --title "v1.0.0" --notes "リリースノート..."

# ビルド成果物を添付してリリース
# → リリースにファイル（配布用 zip など）を添付する
gh release create v1.0.0 ./dist/app.zip

# リリースの詳細
# → バージョン v1.0.0 のリリース情報を表示する
gh release view v1.0.0
```

---

### 4.6 GitHub Actions の確認

> **【補足】GitHub Actions とは？**
> GitHub Actions は、コードを push したり PR を作成したりしたときに、テストやビルドなどを**自動で実行**してくれる仕組みです。
> 各実行には「run-id」と呼ばれる固有のIDが付きます。`gh run list` で一覧を確認すると ID がわかります。
> ワークフロー（workflow）とは、「何をどのタイミングで実行するか」を定義したファイル（`.yml`）のことです。

```bash
# ワークフローの実行履歴
# → これまでの自動実行の一覧（成功・失敗・実行中など）を表示する
gh run list

# 特定の実行の詳細・ログ
# → 指定した run-id の実行結果を確認する
gh run view <run-id>
# → 詳細なログ（エラー原因など）も含めて表示する
gh run view <run-id> --log

# 失敗した実行を再実行
# → 失敗した run-id のワークフローをもう一度実行する
gh run rerun <run-id>

# ワークフローを手動実行
# → 指定したワークフローファイルを手動でトリガーする
gh workflow run <workflow.yml>
```

---

### 4.7 Gist（コード断片の共有）

> **【補足】Gist とは？**
> Gist（ギスト）は、GitHub が提供するコードスニペット（断片）の共有サービスです。
> 1〜数ファイルのメモや小さなスクリプトをすばやく共有したいときに使います。
> 「公開（`--public`）」にすると誰でも URL でアクセスでき、デフォルト（オプションなし）は**非公開**です。

```bash
# ファイルから Gist を作成（公開）
# → script.sh の内容を公開 Gist として GitHub にアップロードする
gh gist create script.sh --public

# 非公開 Gist
# → notes.md の内容を自分だけが見られる非公開 Gist として作成する
gh gist create notes.md

# Gist 一覧
# → 自分が作成した Gist の一覧を表示する
gh gist list
```

---

## 5. 便利な使い方・Tips

**任意の `gh` コマンドをブラウザで開く**

多くの `view` 系コマンドは `--web` を付けるとブラウザで開けます。

```bash
gh repo view --web
gh pr view 45 --web
```

**API を直接叩く**

`gh api` で GitHub REST / GraphQL API を直接呼び出せます。

```bash
gh api repos/mftok89/obsidian
gh api user
```

**エイリアス（コマンドの短縮登録）**

```bash
# 例: gh co を pr checkout の別名にする
gh alias set co 'pr checkout'
gh co 45
```

**拡張機能（extension）**

コミュニティ製の便利機能を追加できます。

```bash
gh extension install <owner/repo>
gh extension list
gh extension upgrade --all
```

**出力をスクリプトで使う（JSON 出力）**

```bash
gh pr list --json number,title,author
```

**設定の確認・変更**

```bash
gh config list
gh config set editor "code --wait"   # 既定エディタを VS Code に
```

---

## 6. トラブルシューティング

| 症状                              | 対処                                                     |
| ------------------------------- | ------------------------------------------------------ |
| `gh` が見つからない（command not found） | ターミナルを開き直す。パスが通っているか確認。再インストールも検討。                     |
| 認証エラーが出る                        | `gh auth status` で状態確認。必要なら `gh auth login` で再ログイン。    |
| `git push` で認証を求められる            | `gh auth setup-git` を実行して Git 認証ヘルパーを設定。               |
| 権限不足（403 / scope エラー）           | `gh auth refresh -s <必要なスコープ>` で権限を追加。                 |
| 社内 GitHub Enterprise に接続したい     | `gh auth login` でホスト名を指定。または `GH_HOST` 環境変数を設定。        |
| バージョンが古い                        | 各 OS のアップデートコマンドで更新（[2.5 インストールの確認](#25-インストールの確認)参照）。 |

ヘルプはいつでも次で確認できます。

```bash
gh help
gh <コマンド> --help     # 例: gh pr --help
```

---

## 7. チートシート（よく使うコマンド一覧）

```text
# 認証
gh auth login                 ログイン
gh auth status                認証状態の確認
gh auth setup-git             Git 認証ヘルパー設定

# リポジトリ
gh repo clone mftok89/obsidian  クローン
gh repo create                新規作成
gh repo view --web            ブラウザで開く

# Issue
gh issue list                 一覧
gh issue create               作成
gh issue view <番号>          詳細
gh issue close <番号>         クローズ

# プルリクエスト
gh pr create --fill           PR 作成（自動入力）
gh pr list                    一覧
gh pr checkout <番号>         チェックアウト
gh pr checks                  CI 状況
gh pr review --approve        承認
gh pr merge --squash          マージ

# リリース / Actions
gh release create <tag>       リリース作成
gh run list                   Actions 実行履歴
gh run view <id> --log        ログ確認

# その他
gh api <endpoint>             API 直接呼び出し
gh alias set <名前> <コマンド> エイリアス登録
gh extension install <repo>   拡張機能追加
```

---

### 参考リンク

- 公式サイト: <https://cli.github.com>
- コマンドリファレンス（マニュアル）: <https://cli.github.com/manual>
- ソース・リリース: <https://github.com/cli/cli/releases>
