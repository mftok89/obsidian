---
title: "スマホからのリモートコントロールobsidian"
created: 2026-06-09
tags: []
---

https://www.youtube.com/watch?v=lUE5MF06zIU

# Claude remote-control 導入手順まとめ（Obsidian活用向け）

## これでできること

- 自宅PCの Claude + Obsidian 環境を
    - スマホ
    - iPad
    - ノートPC
    - ブラウザ  
        からそのまま使える
- 例えば…
    - ベッドでアイデア整理
    - 電車で続きを作業
    - カフェで気分転換
    - 散歩しながら音声入力
    - ソファで振り返り

が可能になる。

---

# 必要な前提

## 必須

- Claude Code（CLI版）
- ターミナル操作が少しできる
- Obsidian がPCに入っている

## 注意

これは「Claude Desktop」ではなく、  
**Claude Code（CLI版）** を使う機能。

---

# 最初にやること（超重要）

## ① ターミナルで Claude を起動

例：

```
claude
```

---

# ② remote-control を有効化

Claude セッション中で：

```
/remote-control
```

を実行。

---

# ③ 「1」を選ぶ

最初だけ質問される。

```
1. Same directory2. Workspace
```

→ 普通は `1`

これで現在のObsidian環境をそのまま共有できる。

---

# ④ QRコードをスマホで読む

すると：

- QRコード  
    または
- URL

が表示される。

スマホの Claude アプリで読み込む。

---

# ⑤ スマホ側でセッション確認

Claudeアプリの：

```
Code↓Sessions
```

に、PCのセッションが出現。

これで接続完了。

---

# 毎回 `/remote-control` を打ちたくない場合

## 自動化設定（おすすめ）

Claude内で：

```
/config
```

を実行。

---

## 検索欄で

```
remote
```

と入力。

---

## 以下を ON にする

```
Enable remote control for all sessions
```

↓

```
true
```

に変更。

---

# これで起きること

今後は：

```
claude
```

で起動した全セッションが、  
自動でスマホやブラウザから見える。

かなり便利。

---

# 実際のおすすめ運用

## 朝

PCでClaude起動しておく。

---

## 外出中

スマホで：

- アイデア追加
- 音声入力
- Obsidian更新

---

## カフェ

iPadや軽量ノートPCから続き作業。

---

## 夜

ソファで振り返り。

---

# 使えるもの

そのまま利用可能：

- Obsidianファイル
- カスタムスキル
- MCP
- claude.md
- ローカルファイル

つまり：

「家PCそのもの」

を触っている感覚。

---

# 注意点（重要）

## PCがスリープすると止まる

### 動く

- 画面ロック
- ディスプレイOFF

### 動かない

- スリープ
- PCを閉じる
- シャットダウン

---

# Mac のおすすめ設定

## システム設定 → バッテリー

- スリープしない
- ディスプレイのみOFF

に近づける。

---

# 接続直後の注意

最初は：

```
/slash command
```

が効かない場合がある。

その時は：

```
こんにちは
```

など1回送信する。

すると：

- スキル
- MCP
- コマンド

が同期される。

---

# スマホでの注意

スマホ版Claudeは：

- スラッシュコマンド補完  
    が弱い。

なので：

```
○○スキルを使って
```

と文章で指示する。

---

# この機能が向いている人

特に相性がいいのは：

- Obsidian 活用者
- アイデア整理が多い人
- Claude Code利用者
- 散歩中に考える人
- 「PC前固定」が辛い人

です。

---

# 最低限これだけ覚えればOK

## 初回

```
/remote-control
```

↓

QR読む

↓

スマホ接続

---

## 永続化

```
/config
```

↓

```
Enable remote control for all sessions = true
```

これで完成です。