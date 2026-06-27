---
title: "Gemini を Claude のサブエージェントに ― WSL2 実装ガイド"
tags: [AI, Claude, Gemini, WSL2, 実装メモ]
source: "Google Cloud Japan ブログ (2026-06-25, Yuting Lin)"
---

# Gemini を Claude のサブエージェントにする ― WSL2 実装ガイド

## この構成でできること

Claude Code（指揮役）が判断・設計を担い、Gemini（実行役）が定型作業・大量処理・Web検索をこなす **2層構造**を作る。

- **コスト削減**：同品質のまま Claude の API コストを 27〜64% 削減（大規模タスク時）
- **Web リサーチ**：Gemini が検索 → Claude が裏取り・統合する定型化されたリサーチフロー
- **クロスモデル検証**：diff を Gemini が独立レビュー → Claude が統合

> **小さな単発タスクは Claude 単独の方が安い。** まとまった量の仕事（大規模機能実装・一括移行・テスト生成・大量ドキュメント調査）でこそ効果が出る。

---

## 登場するツール

| ツール | 役割 |
|---|---|
| **Claude Code** | 指揮役。判断・設計・検証 |
| **Antigravity CLI (`agy`)** | 実行役。Gemini で動く Google の CLI エージェント |
| **Antigravity Plugin** | Claude Code から `agy` を呼ぶための橋渡し |

---

## 準備するもの

### 1. アカウント・サービス

- [ ] Google アカウント
- [ ] Google Cloud プロジェクト（新規作成でも可）
- [ ] Vertex AI API の有効化（後述）

### 2. WSL2 側に必要なもの

- [ ] Ubuntu 上で `gcloud` CLI が使える状態
- [ ] `go` (Go言語ランタイム) または `npm` ※ agy のインストール方法による
- [ ] Claude Code インストール済み

---

## 実装プラン

### Phase 1：Google Cloud の準備

#### 1-1. Google Cloud プロジェクトの作成と課金設定

```bash
# ブラウザで https://console.cloud.google.com を開き
# 新しいプロジェクトを作成する（例：claude-gemini-agent）
# 課金アカウントをリンクする（Vertex AI は有料 API）
```

> **費用感**：Gemini 1.5 Flash は安価。小〜中規模の実験なら月数百円程度の見込み。

#### 1-2. Vertex AI API の有効化

```bash
gcloud services enable aiplatform.googleapis.com --project=YOUR_PROJECT_ID
```

#### 1-3. gcloud CLI のインストール（WSL2 / Ubuntu）

```bash
# gcloud がまだ入っていない場合
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init
```

認証：

```bash
gcloud auth login
gcloud auth application-default login
gcloud config set project YOUR_PROJECT_ID
```

---

### Phase 2：Antigravity CLI (`agy`) のインストール

> agy は Go 製（Node.js 不要）。インストール方法は公式ドキュメントを確認する。
> 2026-06 時点の推定コマンド：

```bash
# 方法A: npm 経由（要確認）
npm install -g @google/antigravity

# 方法B: Go 経由（要確認）
go install github.com/google/antigravity/cmd/agy@latest
```

インストール確認：

```bash
agy --version
```

認証確認（gcloud のログインと共用される）：

```bash
agy --print "hello"
```

---

### Phase 3：Claude Code Plugin のインストール

Claude Code のターミナル内で以下を実行：

```
/plugin marketplace add yuting0624/antigravity-for-claude-code
/plugin install antigravity@antigravity-for-claude-code
```

---

### Phase 4：環境チェックと動作確認

```
/antigravity:setup
```

`doctor` が走り、未設定の項目を教えてくれる。表示された警告をひとつずつ対処する。

---

## WSL2 特有の注意点（重要）

### リポジトリは Linux FS 側に置く

`/mnt/c/...` のような Windows ドライブマウントに作業リポジトリを置くと、`agy --add-dir` の I/O が **9p プロトコル経由で極端に遅くなる**。

```bash
# NG（遅い）
/mnt/c/Users/osamu/projects/myapp

# OK（速い）
~/projects/myapp
```

Obsidian Vault（`G:/マイドライブ/...`）は参照専用として使い、**コーディング作業は `~/projects/` 以下で行う**。

---

## 使い方（スラッシュコマンド）

| コマンド | 何をするか |
|---|---|
| `/antigravity:setup` | 環境チェック（初回・更新後に実行） |
| `/antigravity:delegate <タスク>` | 定型タスクを Gemini に委譲 → Claude が検証 |
| `/antigravity:delegate --tier flash <タスク>` | 安価な Flash モデルで委譲 |
| `/antigravity:review` | 現在の diff を Gemini が独立レビュー → Claude が統合 |
| `/antigravity:research <トピック>` | Gemini が検索 → Claude が2ソース裏取り |
| `/antigravity:status` | バックグラウンドジョブの確認 |

### 委譲に向いているタスク例

- README・コメントの一括生成
- テストコードの網羅的な生成
- 大量ファイルのリファクタリング
- 技術調査・最新情報の収集

---

## 効果を最大化する4つのコツ

1. **まとまった量の仕事に使う**：小タスクは Claude 直接の方が安い
2. **agy の出力は要約だけ受け取る**：全文を Claude のコンテキストに戻さない（コスト増の主因）
3. **複数タスクはまとめて一度に投げる**：往復回数を減らす
4. **レビューは diff だけ渡す**：全文でなく差分で

---

## 実装チェックリスト

- [ ] Google Cloud プロジェクト作成・課金設定
- [ ] Vertex AI API 有効化
- [ ] `gcloud auth login` 完了
- [ ] `agy --version` で動作確認
- [ ] Claude Code Plugin インストール
- [ ] `/antigravity:setup` でエラーなし
- [ ] `/antigravity:delegate "hello world を出力するPythonを書いて"` で動作確認
- [ ] 作業リポジトリを Linux FS 側（`~/projects/`）に移動済み

---

## 参考リンク

- Plugin リポジトリ: `github.com/yuting0624/antigravity-for-claude-code`
- 元記事: Google Cloud Japan ブログ (Yuting Lin, 2026-06-25)
- ライセンス: MIT（コミュニティプロジェクト、Google / Anthropic 非公式）
