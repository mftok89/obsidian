---
title: "00claude tips"
created: 2026-05-13
tags: []
---

https://www.youtube.com/watch?v=tckOndDPhm8&t=732s

 Tips⑦：Auto Run Mode[17:28](https://www.youtube.com/watch?v=tckOndDPhm8&t=1048s)
 Tips⑧：/fewer-permission-prompts , /permissions [19:02](https://www.youtube.com/watch?v=tckOndDPhm8&t=1142s) 
 
### バージョン確認
	バージョン確認及びアップデートコマンド
```version
claude update
```
	バージョンアップ版のインストール
```
claude install
```


### /model
モデルのレベルの確認と設定ができる
  Select model
  Switch between Claude models. Applies to this session and future Claude Code sessions. For other/previous model
  names, specify with --model.

  > 1. Default (recommended) √  Sonnet 4.6 · Best for everyday tasks
    2. Opus                     Opus 4.7 · Most capable for complex work · ~2× usage vs Sonnet
    3. Haiku                    Haiku 4.5 · Fastest for quick answers

  ● High effort (default) ←/→ to adjust
  Enter to confirm · Esc to cancel
  ※　左右ボタンで、/effortを兼ねることができる　
#### /effort
左右ボタンで、effortのレベルの設定を変更できる
	 Speed                         Intelligence
	 ───────────────▲───────────
	            low     medium     high     xhigh      max
	            
	 ←/→ to adjust · Enter to confirm · Esc to cancel
※/model　でも左右ボタンで、このコントロールができる### /plan
	仕様書を作るときなど、planモードで動かすことができる

### /btw
	claudeが動いていて、待ち時間に、ちょっとした質問をすることができる。
	終わりたいときは、escキーで終わることができる。
	コンテキストを邪魔しないため、小休止とかちょっと調べたいときに使えるコマンド

### /resume
	新しく立ち上げたclaudeに、過去のセッションの会話をさかのぼらせて、続きから始めることができる

### /clear
	セッションをクリアして、新規にコンテキストを始められる

### /rename
	セッション名を変更できる

### claude --continue
	前のセッションを引き継いだ状態で、claudeが立ち上がる


### /rewind
	ひとつ前に巻き戻すことができる　codeも戻すことができるし、会話も戻すことができる
	※　escキー２回押しでもできる

### ! npm run dev
ターミナル上で頭に!をつけることで、ローカルサーバーを立ち上げてWEB確認ができるようになる


### /batchとGit Worktree
	同じ作業を並列で行う。/batchの後に行ってもらうことを付け加える
	自動的にgit worktreeが立ち上がって、並列で実行してくれる

### /insights
	claude codeの使い方の分析、改善案を出してくれる