---
title: "SYNC_SETUP"
created: 2026-05-02
tags: []
---

# GitHub ↔ Google Drive 同期 セットアップ手順

## 概要

GitHub Actions を使って、GitHubリポジトリとGoogle Driveフォルダを手動で同期します。
同期ツールには [rclone](https://rclone.org/) を使用します。

---

## STEP 1: ワークフローファイルをリポジトリに追加

1. リポジトリ `mftok89/obsidian_git` をローカルに clone（またはすでにある場合はそちらを使用）
2. リポジトリのルートに `.github/workflows/` フォルダを作成
3. `sync-google-drive.yml`（このファイルと一緒に配布）をそのフォルダに配置
4. コミット＆プッシュ

```
obsidian_git/
└── .github/
    └── workflows/
        └── sync-google-drive.yml   ← ここに配置
```

---

## STEP 2: rclone をインストールして Google Drive を設定

### Windows の場合

1. [https://rclone.org/downloads/](https://rclone.org/downloads/) から Windows 版をダウンロード・インストール
2. コマンドプロンプトまたは PowerShell を開き、以下を実行:

```
rclone config
```

3. 対話形式で設定を進めます:
   - `n` → 新しいリモートを作成
   - 名前: `gdrive`（任意）
   - ストレージタイプ: `drive`（Google Drive）
   - Client ID / Secret: **空白のままEnter**（デフォルト使用）
   - スコープ: `1`（full access）
   - ブラウザが開くので Google アカウントにログイン・許可
   - `y` → 設定完了

4. 設定内容を確認:

```
rclone config show
```

出力例:
```
[gdrive]
type = drive
token = {"access_token":"..."}
```

### Mac の場合

```bash
brew install rclone
rclone config
# → 上記と同様の手順
```

---

## STEP 3: rclone 設定を Base64 エンコード

設定ファイルを GitHub Secrets に保存するため、Base64 に変換します。

### Windows (PowerShell)
```powershell
$config = Get-Content "$env:APPDATA\rclone\rclone.conf" -Raw
[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($config))
```

### Mac / Linux
```bash
base64 ~/.config/rclone/rclone.conf
```

出力された長い文字列（Base64）をコピーしておきます。

---

## STEP 4: GitHub Secrets を設定

1. GitHub で `https://github.com/mftok89/obsidian_git` を開く
2. **Settings** → **Secrets and variables** → **Actions** を開く
3. **New repository secret** をクリックし、以下の2つを追加:

| Secret 名 | 値 |
|---|---|
| `RCLONE_CONFIG_BASE64` | STEP 3 でコピーした Base64 文字列 |
| `GDRIVE_FOLDER_ID` | `1MMcmVXqDDNJEbD5avbKEO8RQYRe3_8m6` |

> **GDRIVE_FOLDER_ID** は Google Drive の URL から取得できます:
> `https://drive.google.com/drive/folders/【ここの文字列】`

---

## STEP 5: 初回同期を実行

1. GitHub の **Actions** タブを開く
2. 左側から **「Google Drive と同期」** ワークフローを選択
3. **Run workflow** ボタンをクリック
4. 同期方向を選択して実行:

| 選択肢 | 説明 |
|---|---|
| `bidirectional` | 両方向で差分を同期（初回はこれを推奨） |
| `github-to-drive` | GitHub の内容で Drive を上書き |
| `drive-to-github` | Drive の内容で GitHub を上書き |

> **初回は `bidirectional` を選んでください。**
> 内部的に `--resync` フラグが付いており、同期の基準点を作成します。

---

## 2回目以降の注意点

初回以降、`bidirectional` モードを安定して使うには、ワークフローの以下の行を**削除**してください（`--resync` は初回のみ必要）:

```yaml
            --resync \
```

`--resync` を毎回付けると、どちらか一方を正とみなして上書きする場合があります。

---

## 同期のしくみ

```
Obsidian（PC）
    ↕ obsidian-git プラグイン
GitHub リポジトリ
    ↕ このワークフロー（手動実行）
Google Drive フォルダ
```

---

## トラブルシューティング

- **Actions タブにワークフローが表示されない**: `.github/workflows/` の場所とファイル名を確認
- **rclone 認証エラー**: Secrets の `RCLONE_CONFIG_BASE64` が正しくエンコードされているか確認
- **Drive フォルダが見つからない**: `GDRIVE_FOLDER_ID` が正しいか確認
- **bisync エラー**: 初回は必ず `--resync` が必要。ワークフローログでエラー内容を確認
