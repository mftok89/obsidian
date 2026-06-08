# WSL2 + Cursor + GitHub 完全セットアップガイド
> Panasonic PC (Windows 11) 向け — 環境構築から日常的なGit操作まで

---

## 目次

1. [WSL2 の有効化とインストール](#1-wsl2-の有効化とインストール)
2. [Ubuntu の初期設定](#2-ubuntu-の初期設定)
3. [開発ツールのインストール](#3-開発ツールのインストール)
4. [Git の設定](#4-git-の設定)
5. [SSH鍵の生成とGitHubへの登録](#5-ssh鍵の生成とgithubへの登録)
6. [Cursor のインストールと WSL2 連携](#6-cursor-のインストールと-wsl2-連携)
7. [Cursor から GitHub を操作する](#7-cursor-から-github-を操作する)
8. [日常的な Git ワークフロー](#8-日常的な-git-ワークフロー)
9. [よくあるエラーと対処法](#9-よくあるエラーと対処法)
10. [便利なエイリアス・設定集](#10-便利なエイリアス設定集)

---

## 1. WSL2 の有効化とインストール

### 前提条件
- Windows 11 (または Windows 10 バージョン 2004 以降)
- 管理者権限のあるアカウント

### 手順

#### ① PowerShell を管理者として開く
`スタートメニュー` → `PowerShell` を右クリック → `管理者として実行`

#### ② WSL2 と Ubuntu を一括インストール
```powershell
wsl --install
```
> 自動的に WSL2 が有効化され、Ubuntu がインストールされます。  
> インストール完了後、**PCを再起動**してください。

#### ③ WSL のバージョン確認
```powershell
wsl --list --verbose
```
出力例：
```
  NAME      STATE           VERSION
* Ubuntu    Running         2
```
`VERSION` が `2` であることを確認する。

#### ④ 既存の WSL が Version 1 の場合（バージョンアップ）
```powershell
wsl --set-version Ubuntu 2
wsl --set-default-version 2
```

---

## 2. Ubuntu の初期設定

再起動後、Ubuntu が自動起動し、ユーザー名とパスワードの設定を求められる。

```
Enter new UNIX username: （任意のユーザー名を入力）
New password: （任意のパスワードを入力 ※入力中は表示されない）
Retype new password: （パスワードを再入力）
```

### パッケージの更新
```bash
sudo apt update && sudo apt upgrade -y
```

### 日本語ロケールの設定（任意）
```bash
sudo apt install -y language-pack-ja
sudo update-locale LANG=ja_JP.UTF-8
```

---

## 3. 開発ツールのインストール

### 基本的なビルドツール
```bash
sudo apt install -y build-essential curl wget git unzip
```

### Node.js（nvm 経由が推奨）
```bash
# nvm のインストール
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# シェルを再読み込み
source ~/.bashrc

# 最新の LTS をインストール
nvm install --lts
nvm use --lts

# バージョン確認
node -v
npm -v
```

### Python
```bash
sudo apt install -y python3 python3-pip python3-venv
python3 --version
```

### その他（必要に応じて）
```bash
# Docker (WSL2 上で使う場合)
sudo apt install -y docker.io
sudo usermod -aG docker $USER

# zsh + oh-my-zsh（任意）
sudo apt install -y zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

---

## 4. Git の設定

```bash
# ユーザー名・メールアドレスの設定（GitHub アカウントに合わせる）
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

# デフォルトブランチ名を main に設定
git config --global init.defaultBranch main

# 日本語ファイル名を正しく表示
git config --global core.quotepath false

# 改行コードの自動変換を無効化（WSL2 推奨設定）
git config --global core.autocrlf false

# 設定確認
git config --global --list
```

---

## 5. SSH鍵の生成とGitHubへの登録

HTTPS よりも SSH 接続が安定していておすすめです。

### ① SSH 鍵ペアの生成
```bash
ssh-keygen -t ed25519 -C "you@example.com"
```
プロンプトが出たら全て Enter でスキップ（パスフレーズは任意）。

### ② 公開鍵の確認
```bash
cat ~/.ssh/id_ed25519.pub
```
`ssh-ed25519 AAAA...` から始まる1行をコピーする。

### ③ GitHub に公開鍵を登録
1. [GitHub → Settings → SSH and GPG keys](https://github.com/settings/keys) を開く
2. `New SSH key` をクリック
3. `Title`：任意（例：`Panasonic WSL2`）
4. `Key`：コピーした公開鍵を貼り付け
5. `Add SSH key` をクリック

### ④ 接続テスト
```bash
ssh -T git@github.com
```
成功時の表示：
```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 6. Cursor のインストールと WSL2 連携

### ① Cursor を Windows にインストール
[https://cursor.sh](https://cursor.sh) からダウンロードしてインストールする。

### ② WSL Extension の確認
Cursor 起動後、左下の `><` アイコン（リモート接続ボタン）をクリックし、  
**「WSL への接続」** または **「Connect to WSL」** を選択する。

### ③ WSL2 内のプロジェクトを開く方法

**方法A: Cursor のターミナルから**
```bash
# WSL のターミナルでプロジェクトフォルダに移動し
cd ~/projects/my-app

# cursor コマンドで開く
cursor .
```

**方法B: Cursor の「フォルダを開く」から**
- `ファイル` → `フォルダを開く`
- パスを `\\wsl$\Ubuntu\home\ユーザー名\projects\my-app` と入力

### ④ WSL2 上に cursor コマンドをインストール（初回のみ）
Cursor のコマンドパレット（`Ctrl + Shift + P`）を開き、  
`Shell Command: Install 'cursor' command in PATH` を実行する。

---

## 7. Cursor から GitHub を操作する

### ソース管理パネルを使う（GUI操作）

左サイドバーの **ブランチアイコン**（ソース管理）をクリックすると、変更ファイルの一覧が表示される。

| 操作 | Cursor での手順 |
|------|----------------|
| **ステージング** | 変更ファイルの `+` ボタンをクリック |
| **コミット** | メッセージ入力欄に記入 → `✓ コミット` ボタン |
| **プッシュ** | `...` メニュー → `プッシュ` または 下部バーの `↑` ボタン |
| **プル** | `...` メニュー → `プル` または 下部バーの `↓` ボタン |
| **ブランチ作成** | 左下のブランチ名をクリック → `新しいブランチを作成` |

### Cursor 内蔵ターミナルを使う（コマンド操作）

ターミナルは `` Ctrl + ` `` で開ける。WSL2 の bash/zsh が起動する。

---

## 8. 日常的な Git ワークフロー

### 🆕 新しいリポジトリを作成してプッシュ

```bash
# プロジェクトフォルダを作成
mkdir ~/projects/my-app && cd ~/projects/my-app

# Git を初期化
git init

# ファイルを作成
echo "# My App" > README.md

# ステージング
git add .

# コミット
git commit -m "first commit"

# GitHub でリポジトリを作成後、リモートを追加（SSH URL を使う）
git remote add origin git@github.com:ユーザー名/リポジトリ名.git

# プッシュ
git push -u origin main
```

---

### 📥 既存のリポジトリをクローン

```bash
cd ~/projects

# SSH でクローン（推奨）
git clone git@github.com:ユーザー名/リポジトリ名.git

# フォルダに移動
cd リポジトリ名

# Cursor で開く
cursor .
```

---

### 💾 変更をコミット・プッシュ（日常作業）

```bash
# 変更状況を確認
git status

# 変更差分を確認
git diff

# 全ての変更をステージング
git add .

# 特定ファイルだけステージング
git add src/index.js

# コミット
git commit -m "feat: ログイン機能を追加"

# プッシュ
git push
```

---

### 📤 最新の変更を取得（プル）

```bash
# リモートの変更を取得してマージ
git pull

# リベース方式で取得（履歴をきれいに保ちたい場合）
git pull --rebase
```

---

### 🌿 ブランチ操作

```bash
# ブランチ一覧を確認
git branch -a

# 新しいブランチを作成して切り替え
git switch -c feature/新機能名

# ブランチを切り替え
git switch main

# ブランチをマージ（main に feature を取り込む）
git switch main
git merge feature/新機能名

# 不要なブランチを削除
git branch -d feature/新機能名

# リモートにブランチをプッシュ
git push -u origin feature/新機能名
```

---

### 🔄 コミット履歴の確認

```bash
# シンプルなログ
git log --oneline

# グラフ付きで表示
git log --oneline --graph --all

# 直近5件だけ表示
git log --oneline -5
```

---

### ↩️ 変更を取り消す

```bash
# ステージングを取り消す（ファイルの変更は残る）
git restore --staged ファイル名

# ファイルの変更を取り消す（変更が消えるので注意）
git restore ファイル名

# 直前のコミットを取り消す（変更は残る）
git reset --soft HEAD~1

# コミットごと完全に取り消す（変更も消える・注意）
git reset --hard HEAD~1
```

---

## 9. よくあるエラーと対処法

### ❌ `Permission denied (publickey)`
SSH 鍵が GitHub に登録されていない、または ssh-agent に鍵が登録されていない。

```bash
# ssh-agent を起動して鍵を追加
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 再度接続テスト
ssh -T git@github.com
```

---

### ❌ `error: remote origin already exists`
すでに origin が登録されている。

```bash
# 既存の origin を削除して再登録
git remote remove origin
git remote add origin git@github.com:ユーザー名/リポジトリ名.git
```

---

### ❌ `Updates were rejected because the tip of your current branch is behind`
リモートの変更を先にプルする必要がある。

```bash
git pull --rebase
git push
```

---

### ❌ `CRLF / LF の警告`
Windows と Linux の改行コードの違い。WSL2 ではすでに `autocrlf false` にしているが、念のため：

```bash
git config --global core.autocrlf false
git config --global core.eol lf
```

---

### ❌ Cursor で WSL2 に接続できない
- Windows の Hyper-V / 仮想マシンプラットフォームが有効か確認
- PowerShell で `wsl --status` を実行して WSL2 の状態を確認
- Cursor を再起動して再接続を試みる

---

## 10. 便利なエイリアス・設定集

### Git エイリアス（~/.gitconfig に追加）

```bash
git config --global alias.st "status"
git config --global alias.co "switch"
git config --global alias.br "branch"
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.cm "commit -m"
git config --global alias.unstage "restore --staged"
```

使い方の例：
```bash
git st        # git status
git lg        # ログをグラフ表示
git cm "fix"  # git commit -m "fix"
```

---

### ~/.bashrc（または ~/.zshrc）に追加すると便利な設定

```bash
# WSL2 起動時に ssh-agent を自動起動
if [ -z "$SSH_AUTH_SOCK" ]; then
  eval "$(ssh-agent -s)" > /dev/null
  ssh-add ~/.ssh/id_ed25519 2>/dev/null
fi

# Git ブランチをプロンプトに表示
parse_git_branch() {
  git branch 2>/dev/null | sed -n 's/* //p'
}
export PS1='\u@\h:\w\[\e[32m\]$([ -n "$(parse_git_branch)" ] && echo " ($(parse_git_branch))")\[\e[0m\]\$ '

# よく使うショートカット
alias gs="git status"
alias ga="git add ."
alias gp="git push"
alias gl="git pull"
alias glog="git log --oneline --graph --all"
```

設定を反映：
```bash
source ~/.bashrc
```

---

### .gitignore のテンプレート

プロジェクトルートに `.gitignore` を作成：

```gitignore
# OS
.DS_Store
Thumbs.db

# エディタ
.vscode/
.cursor/
*.swp

# Node.js
node_modules/
npm-debug.log*

# Python
__pycache__/
*.pyc
.venv/
*.egg-info/

# 環境変数（絶対にコミットしない）
.env
.env.local
.env.*.local

# ビルド成果物
dist/
build/
```

---

## 参考リンク

| リソース | URL |
|----------|-----|
| WSL 公式ドキュメント | https://learn.microsoft.com/ja-jp/windows/wsl/ |
| Cursor 公式サイト | https://cursor.sh |
| GitHub SSH 接続ガイド | https://docs.github.com/ja/authentication/connecting-to-github-with-ssh |
| Git 公式リファレンス | https://git-scm.com/docs |

---

*最終更新: 2026年5月*
