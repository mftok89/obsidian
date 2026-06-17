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

### 4.1 リポジトリ操作

```bash
# リポジトリをクローン（owner/repo 形式でOK）
gh repo clone cli/cli

# 新しいリポジトリを作成（対話形式）
gh repo create

# 現在のリポジトリをブラウザで開く
gh repo view --web

# リポジトリ情報を表示
gh repo view owner/repo

# 自分のリポジトリ一覧
gh repo list
```

### 4.2 Issue 操作

```bash
# Issue 一覧
gh issue list

# 自分にアサインされた Issue のみ
gh issue list --assignee "@me"

# Issue を作成（対話形式。--title / --body で直接指定も可）
gh issue create

# Issue の詳細を表示
gh issue view 123

# ブラウザで開く
gh issue view 123 --web

# コメントを追加
gh issue comment 123 --body "対応しました"

# クローズ / 再オープン
gh issue close 123
gh issue reopen 123
```

### 4.3 プルリクエスト操作

```bash
# 現在のブランチから PR を作成
gh pr create --title "機能Aを追加" --body "概要..."

# 対話形式で作成
gh pr create

# PR 一覧
gh pr list

# PR の詳細・差分を確認
gh pr view 45
gh pr diff 45

# PR をローカルにチェックアウト（レビュー用）
gh pr checkout 45

# CI（チェック）の状況を確認
gh pr checks 45

# レビューを送る
gh pr review 45 --approve
gh pr review 45 --request-changes --body "ここを修正してください"

# マージ（squash マージしてブランチ削除）
gh pr merge 45 --squash --delete-branch
```

### 4.4 ブランチとよくある開発フロー

1 つの機能追加を `gh` で進める典型的な流れです。

```bash
# 1. 作業ブランチを作成して移動（git コマンド）
git switch -c feature/login

# 2. コードを編集してコミット
git add .
git commit -m "ログイン機能を追加"

# 3. リモートへ push
git push -u origin feature/login

# 4. PR を作成
gh pr create --fill          # コミット内容からタイトル・本文を自動入力

# 5. CI とレビューを確認
gh pr checks
gh pr view --web

# 6. 承認後にマージ
gh pr merge --squash --delete-branch
```

> `--fill` はコミットメッセージから PR タイトル・本文を自動生成する便利オプションです。

### 4.5 リリース操作

```bash
# リリース一覧
gh release list

# リリースを作成（タグ v1.0.0 を付与）
gh release create v1.0.0 --title "v1.0.0" --notes "リリースノート..."

# ビルド成果物を添付してリリース
gh release create v1.0.0 ./dist/app.zip

# リリースの詳細
gh release view v1.0.0
```

### 4.6 GitHub Actions の確認

```bash
# ワークフローの実行履歴
gh run list

# 特定の実行の詳細・ログ
gh run view <run-id>
gh run view <run-id> --log

# 失敗した実行を再実行
gh run rerun <run-id>

# ワークフローを手動実行
gh workflow run <workflow.yml>
```

### 4.7 Gist（コード断片の共有）

```bash
# ファイルから Gist を作成（公開）
gh gist create script.sh --public

# 非公開 Gist
gh gist create notes.md

# Gist 一覧
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
gh api repos/cli/cli
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

| 症状 | 対処 |
|------|------|
| `gh` が見つからない（command not found） | ターミナルを開き直す。パスが通っているか確認。再インストールも検討。 |
| 認証エラーが出る | `gh auth status` で状態確認。必要なら `gh auth login` で再ログイン。 |
| `git push` で認証を求められる | `gh auth setup-git` を実行して Git 認証ヘルパーを設定。 |
| 権限不足（403 / scope エラー） | `gh auth refresh -s <必要なスコープ>` で権限を追加。 |
| 社内 GitHub Enterprise に接続したい | `gh auth login` でホスト名を指定。または `GH_HOST` 環境変数を設定。 |
| バージョンが古い | 各 OS のアップデートコマンドで更新（[2.5 インストールの確認](#25-インストールの確認)参照）。 |

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
gh repo clone owner/repo      クローン
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
