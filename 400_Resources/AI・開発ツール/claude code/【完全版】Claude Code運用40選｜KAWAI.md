---
title: "【完全版】Claude Code運用40選｜KAWAI"
source: "https://note.com/kawaidesign/n/nce2f82c62f1f"
author:
  - "[[KAWAI]]"
published: 2026-04-28
created: 2026-04-30
description: "Claude Codeは、便利なチャットではありません。  設定、文脈、検証、自動化、並列化まで設計すると、日々の作業環境そのものになります。  この記事では、手元にあるClaude Code運用メモを土台に、Anthropic公式ドキュメント、Claude Help Center、GitHub Actions、MCP、Hooks、Skills、Subagents、非対話モード、情報収集ワークフローまで整理しました。   この記事は全文無料（期間限定）で閲覧できます。   見出し画像はAIで生成しました。 プロンプトはこの記事に掲載中。     Claude Codeは「会話相手」で"
tags:
  - "clippings"
---
Claude Codeは、便利なチャットではありません。

設定、文脈、検証、自動化、並列化まで設計すると、日々の作業環境そのものになります。

この記事では、手元にあるClaude Code運用メモを土台に、Anthropic公式ドキュメント、Claude Help Center、GitHub Actions、MCP、Hooks、Skills、Subagents、非対話モード、情報収集ワークフローまで整理しました。


## Claude Codeは「会話相手」ではなく「作業環境」です

![画像](https://assets.st-note.com/img/1777363082-li5sCDuqcYkUn29IWVadO6H8.png?width=1200)

Claude Codeを使いこなせない原因の多くは、プロンプトの上手さではありません。作業環境として扱えていないことです。

公式ベストプラクティスでも、Claude Codeはファイルを読み、コマンドを実行し、変更を加え、自律的に問題を進めるエージェント型の開発環境として説明されています。

つまり、ただ質問する道具ではなく、仕事の進め方を設計する対象です。

**押さえる軸は6つです。**

1つ目は、探索と実装を分けること。

2つ目は、テストやスクリーンショットでClaude自身に検証させること。

3つ目は、CLAUDE.mdやSkillsで文脈を外部化すること。

4つ目は、HooksやPermissionsで守るべき動作を仕組みにすること。

5つ目は、Subagentsや複数セッションで作業を並列化すること。

6つ目は、情報収集を一度きりの検索ではなく、蓄積されるナレッジに変えることです。

---




[**【超初心者OK】Claude Code 個別サポート｜【MENTA】No1.メンターサービスでプロに直接相談しよう！** *■ このコースについてClaude Codeを「インストールしただけ」で終わっていませんか？私は毎日Claude Code* *menta.work*](https://menta.work/plan/16678)

## まず入れるべき基本設定

![画像](https://assets.st-note.com/img/1777363091-d38F7zXxV2cEhB0Zwak4DHmf.png?width=1200)

最初にやるべきことは、すごいコマンドを覚えることではありません。

Claudeが迷わず作業できる地図を置くことです。

1. **\`/init\`でCLAUDE.mdを作る**  
	プロジェクトの構造、ビルド方法、テスト方法、コーディング規約を書きます。公式ドキュメントでは、CLAUDE.mdは毎回読み込まれるため、短く、人間が読める形に保つことが推奨されています。
2. **CLAUDE.mdは「同じミスを2回したら追記」する**  
	最初から完璧に書く必要はありません。Claudeが同じ勘違いをしたら、その時点でルール化します。逆に、読まれなくなった長文ルールは削ります。
3. **User memoryとProject memoryを分ける**  
	個人の好みは \`~/.claude/CLAUDE.md\`、チームで共有する規約はプロジェクト直下の \`CLAUDE.md\` に置きます。モノレポでは親ディレクトリと子ディレクトリのCLAUDE.mdを分けると、全体規約と局所規約を両立できます。
4. **\`/memory\`で記憶を確認する**  
	どのメモリが読まれているかを把握せずに運用すると、Claudeがなぜその挙動をしているのか追えません。記憶は便利ですが、増えすぎると逆に効きません。
5. **\`/permissions\`で安全な作業を事前承認する**  
	毎回の許可確認をなくすために、\`npm test\`、\`npm run lint\`、\`git status\` のような安全なコマンドは許可リストに入れます。一方で、\`.env\`、秘密鍵、production設定へのアクセスは明示的に避けます。
6. **settings.jsonをチームで管理する**  
	プロジェクト共通の設定は \`.claude/settings.json\` に置きます。個人差分は \`.claude/settings.local.json\` に逃がすと、チームの作業体験を揃えられます。
7. **\`/statusline\`で現在地を見える化する**  
	ブランチ、モデル、コンテキスト使用率、コストを常に見えるようにします。特に複数セッションや複数worktreeを使う人ほど、現在地の表示が事故防止になります。

## 精度を上げる依頼の出し方

![画像](https://assets.st-note.com/img/1777363108-IMJ1VQn4LzKdfDjoAhrNs75x.png?width=1200)

Claude Codeの精度は、「何を頼むか」より「 **どう検証できる形で頼むか** 」で決まります。

1. **大きい変更はPlan Modeから始める**  
	いきなり実装させず、まず探索、計画、実装、コミットに分けます。公式ベストプラクティスでも、複数ファイルにまたがる変更や不確実な変更ではPlan Modeが推奨されています。
2. **小さい変更はPlan Modeを使わない**  
	誤字修正、ログ追加、1行の条件変更のような作業まで計画させると遅くなります。計画が必要なのは、影響範囲が不明な時です。
3. **成功条件を先に渡す**  
	「直して」ではなく、「このテストが通ること」「このスクリーンショットと差分がないこと」「このCLI出力になること」まで指定します。
4. **Claude自身に検証させる**  
	公式ベストプラクティスは、Claudeに検証手段を与えることを高レバレッジな行動として説明しています。テスト、lint、typecheck、スクリーンショット、期待出力を用意します。
5. **失敗ログは要約せず貼る**  
	エラーメッセージを人間が丸めると、原因の手がかりが消えます。長いログはファイルに保存し、Claudeに読ませます。
6. **既存パターンを指定する**  
	「新しいコンポーネントを作って」ではなく、「既存のWidget実装を読んで、同じパターンで作って」と伝えます。Claude Codeはコードベース内の慣習を読ませた時に強くなります。
7. **仕様が曖昧ならClaudeに質問させる**  
	公式ドキュメントには、Claudeにインタビューさせる使い方も紹介されています。大きな機能ほど、実装前に質問を出させた方が手戻りが減ります。
8. **画像やスクリーンショットを渡す**  
	UI作業では、コードだけでは判断できない崩れが出ます。スクリーンショット、参照画像、ブラウザ確認をセットにすると、Claudeが自分で差分を見つけやすくなります。
9. **\`/rewind\`やチェックポイントを使う**  
	怖い変更は、戻せる前提で試します。ただし、外部API、DB書き込み、メール送信のような副作用はチェックポイントだけでは戻せません。

## 文脈を育てるCLAUDE.md、Skills、Subagents

![画像](https://assets.st-note.com/img/1777363117-nEIYrt7oDLPsuKlm9Qbhp4Nv.png?width=1200)

Claude Codeを毎日使うなら、会話内で説明し続けるのは負けです。繰り返す知識は外に出します。

1. **いつも使う手順はSkillsにする**  
	\`.claude/skills/<name>/SKILL.md\` に、繰り返す作業手順を書きます。公式ベストプラクティスでは、Skillsはプロジェクトやチーム固有の知識、再利用ワークフローを渡す仕組みとして説明されています。
2. **毎回読む必要がない知識はCLAUDE.mdに入れない**  
	CLAUDE.mdは常に読み込まれるため、長くすると効きません。たまに使うドメイン知識、長い手順、チェックリストはSkillsに分けます。
3. **Slash commandは短い定型依頼に使う**  
	\`.claude/commands/\` にMarkdownファイルを置くと、独自のスラッシュコマンドを作れます。引数も渡せるため、\`/fix-issue 123\` のような運用に向いています。
4. **Subagentsは調査、レビュー、デバッグに使う**  
	Subagentsは独立したコンテキストで動く専門エージェントです。公式ドキュメントでは、メイン会話の文脈を汚さず、特定領域に集中できる点が利点として説明されています。
5. **Subagentには役割を絞って書く**  
	万能エージェントを作るより、\`code-reviewer\`、\`debugger\`、\`security-reviewer\`、\`test-runner\` のように責務を分けます。使えるツールも必要最小限にします。
6. **調査はSubagent、意思決定はメインで行う**  
	大量のファイルを読む調査はSubagentに逃がします。ただし、最終判断まで丸投げすると、全体の意図がぼやけます。メインセッションは意思決定の場として残します。
7. **学びはPRコメントからCLAUDE.mdに戻す**  
	レビューで何度も出る指摘は、次回以降のルールにします。コードレビューは、その場の修正だけでなく、未来のClaudeの行動を変える機会です。

## 自動化はHooks、MCP、非対話モードで作る

![画像](https://assets.st-note.com/img/1777363129-6BsNtrUg7YDSH8iyVkQGmCJz.png?width=1200)

Claude Codeの強さは、作業を「お願い」から「仕組み」に変えられることです。

1. **Hooksで例外なく実行する**  
	Hooksは、Claude Codeのライフサイクルに合わせてコマンドを実行する仕組みです。公式ドキュメントでは、\`PreToolUse\`、\`PostToolUse\`、\`UserPromptSubmit\`、\`Stop\`、\`SessionStart\` などのイベントが説明されています。
2. **整形はPostToolUse hookに任せる**  
	ファイル編集後にformatterやlintを走らせると、スタイル崩れを人間が指摘する必要が減ります。ただし、重い処理を毎回走らせると作業全体が遅くなります。
3. **禁止事項はプロンプトではなくHookで止める**  
	「migrationsを勝手に触らないで」と書くより、書き込み前に止めるHookの方が強いです。公式ガイドでも、HooksはLLMの判断に頼らず動作を保証する仕組みとして説明されています。
4. **MCPで外部ツールを同じ作業面に入れる**  
	MCPを使うと、GitHub、Jira、Notion、Figma、DB、SlackなどをClaude Codeから扱えます。公式ドキュメントでは、Issueから実装、監視データ分析、DB照会、Figma連携、Gmail下書き作成などの例が示されています。
5. **MCPの出力はトークン量を管理する**  
	MCPは便利ですが、巨大な出力はコンテキストを圧迫します。必要な範囲だけ取る、ページングする、要約ファイルに落とす、という設計が必要です。
6. **\`claude -p\`で非対話モードにする**  
	\`claude -p "prompt"\` は、CI、pre-commit、バッチ処理、ログ解析に使えます。公式ベストプラクティスでも、\`--output-format json\` や \`stream-json\` による自動処理が紹介されています。
7. **大量ファイルはfan-outする**  
	大規模移行を1セッションに抱えさせるのではなく、対象ファイルのリストを作り、ファイル単位で \`claude -p\` を回します。最初は2、3ファイルで失敗パターンを見てから広げます。
8. **GitHub ActionsでPRやIssueから呼ぶ**  
	Claude Code GitHub Actionsを使うと、PRやIssue上の \`@claude\` メンションから、質問回答、コード変更、PR作成、レビューを実行できます。チーム運用では、ターミナルに閉じない導線が重要です。

## 並列化するとClaude Codeは別物になる

![画像](https://assets.st-note.com/img/1777363183-vb5FhjJYiQ4cCMT7ZsH0t3lo.png?width=1200)

1人で1つのClaudeを眺める使い方から、複数のClaudeを役割分担させる使い方に変えると、体感が変わります。

1. **git worktreeで独立タスクを分ける**  
	認証、UI、テスト修正、ドキュメント更新のように衝突しにくい作業は、worktreeを分けて同時に進めます。レビューできる量を超える本数に増やすと逆効果です。
2. **WriterとReviewerを別セッションにする**  
	同じClaudeに自分の実装をレビューさせるより、別セッションでレビューさせる方がバイアスが減ります。公式ベストプラクティスでも、複数セッションを品質向上に使うWriter/Reviewerパターンが紹介されています。
3. **テストを書くClaudeと実装するClaudeを分ける**  
	先にテストを書くセッション、次に通す実装セッション、最後にレビューするセッションに分けると、仕様の穴が見えやすくなります。
4. **フロントエンドはブラウザ確認まで任せる**  
	Chrome拡張やブラウザ操作を使える環境では、スクリーンショット確認、UI差分、操作確認までセットにします。見た目の作業は、生成だけで終わらせると品質が安定しません。
5. **CloudやDesktopの複数セッションも使い分ける**  
	公式ベストプラクティスでは、Desktop、Web、Agent teamsなど複数セッションの選択肢が紹介されています。ローカルで十分な作業と、クラウド上の隔離環境で進める作業を分けます。
6. **自動承認は安全設計とセットで使う**  
	Auto modeやSandboxingは便利ですが、何でも通すための機能ではありません。許可リスト、deny、sandbox、Hookを組み合わせて、止めるべき操作が止まる状態を作ります。

## 情報収集は「調べる」から「貯まる」に変える

![画像](https://assets.st-note.com/img/1777363196-kX60EigzCL9rIyx7bQconHqw.png?width=1200)

Claude Codeの情報収集は、一回の検索で終わらせると弱いです。

強いのは、調べた内容が次回以降の判断に使われる状態です。

1. **まず普通にClaude Codeに検索させる**  
	最新情報、公式ドキュメント、GitHubリポジトリ、リリースノートを調べるだけなら、普通の検索で十分な場面が多いです。最初から複雑な仕組みにしない方が続きます。
2. **SNSの温度感は専用リサーチに分ける**  
	Reddit、X、YouTube、TikTok、Hacker Newsなどを横断する \`/last30days\` 型の調査。こうした横断検索は、公式情報では見えない「現場の温度感」を取る時に効きます。
3. **Routines、Scheduled tasks、Grok、Obsidianで蓄積する**  
	毎日見るサイトはRoutinesやScheduled tasksで巡回し、Xのリアルタイム性はGrokで拾い、MarkdownでObsidianに保存します。Routinesは公式ドキュメント上でresearch previewとされているため、仕様変更を前提に扱います。最後にCLAUDE.mdやSkill GraphsからClaude Codeが読める状態にすると、「調べる、貯める、使う」のループになります。

## まずはこの順番で入れてください

全部を一気に入れる必要はありません。順番を間違えないことが大事です。

最初の1週間は、 [CLAUDE.md](http://claude.md/) 、Plan Mode、自己検証だけで十分です。ここで「毎回同じ説明をしなくていい」「テストまで自分で回す」状態を作ります。

次の1週間で、Permissions、Hooks、Skillsを入れます。繰り返す作業、守らせたいルール、毎回使うチェックリストを外部化します。

その次に、Subagents、worktree、\`claude -p\`、MCP、GitHub Actionsに広げます。ここからClaude Codeは単体ツールではなく、チームや業務プロセスに組み込む道具になります。

最終的には、情報収集も同じです。検索して終わりではなく、蓄積して、次の投稿、次の記事、次の実装、次の研修に使える形にします。

---

## 参考にした一次情報

- Anthropic / Claude Code Docs「Best Practices for Claude Code」  
	[https://code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices)
- Anthropic / Claude Code Docs「How Claude remembers your project」  
	[https://code.claude.com/docs/en/memory](https://code.claude.com/docs/en/memory)
- Anthropic / Claude Code Docs「Hooks reference」  
	[https://code.claude.com/docs/en/hooks](https://code.claude.com/docs/en/hooks)
- Anthropic / Claude Code Docs「Connect Claude Code to tools via MCP」  
	[https://code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp)
- Anthropic / Claude Code Docs「Claude Code GitHub Actions」  
	[https://code.claude.com/docs/en/github-actions](https://code.claude.com/docs/en/github-actions)
- Claude Help Center「Claude Code power user tips」  
	[https://support.claude.com/en/articles/14554000-claude-code-power-user-tips](https://support.claude.com/en/articles/14554000-claude-code-power-user-tips)
- Anthropic / Claude Code Docs「Run prompts on a schedule」  
	[https://code.claude.com/docs/en/scheduled-tasks](https://code.claude.com/docs/en/scheduled-tasks)
- Anthropic / Claude Code Docs「Automate work with routines」  
	[https://code.claude.com/docs/en/web-scheduled-tasks](https://code.claude.com/docs/en/web-scheduled-tasks)
- X Help Center「About Grok」  
	[https://help.x.com/en/using-x/about-grok](https://help.x.com/en/using-x/about-grok)

---

**Claude Code 知らないと損する40のワザ**  
先着100名限定ウェビナー 詳細はこちら↓

<iframe height="210" data-src="https://note.com/embed/notes/n1c372c70af6f" src="https://note.com/embed/notes/n1c372c70af6f"></iframe>

⬇️ 1on1で密に教えて欲しい方はこちらがオススメです。

**【超初心者OK】Claude Code 個別サポート**  
※早割先着10名様分は完売しました。

[**【超初心者OK】Claude Code 個別サポート｜【MENTA】No1.メンターサービスでプロに直接相談しよう！** *■ このコースについてClaude Codeを「インストールしただけ」で終わっていませんか？私は毎日Claude Code* *menta.work*](https://menta.work/plan/16678)

⬇️ 法人研修をご希望の方はこちら

**Claude Code 法人研修 無料相談はこちら**

[**KAWAI DESIGN | 川合卓也 公式サイト** *AI × デザインでビジネスに「熱」と「純度」を。株式会社SHIFT AI デザイン部長、川合卓也のオフィシャルサイト。* *kawai-official.pages.dev*](https://kawai-official.pages.dev/)

noteメンバーシップに参加すると700本以上の記事が読み放題です。

[**KAWAI BOOKS｜KAWAI** *600本以上の記事が読み放題。AI × デザインを主軸に、独自の視点やノウハウをお届けします。AI時代にキャリアアップ、副* *note.com*](https://note.com/kawaidesign/membership)

お問い合わせ・個別相談・法人研修のご依頼

[**KAWAI DESIGN | 川合卓也 公式サイト** *AI × デザインでビジネスに「熱」と「純度」を。株式会社SHIFT AI デザイン部長、川合卓也のオフィシャルサイト。* *kawai-official.pages.dev*](https://kawai-official.pages.dev/)

AI活用・キャリア戦略 個別相談

[**AI活用・キャリア戦略 個別相談｜【MENTA】No1.メンターサービスでプロに直接相談しよう！** *■ このコースについてAIを学んでも、仕事が変わらない。情報収集はしている。ツールも触った。でも「自分の仕事にどう使うか」* *menta.work*](https://menta.work/plan/16538)

書籍「AIでゼロからデザイン」好評発売中

[**AIでゼロからデザイン** *www.amazon.co.jp*](https://www.amazon.co.jp/dp/4798193429?tag=note0e2a-22&linkCode=ogi&th=1&psc=1&language=ja_JP)

[*2,310円* (2026年04月28日 10:38時点](https://www.amazon.co.jp/dp/4798193429?tag=note0e2a-22&linkCode=ogi&th=1&psc=1&language=ja_JP)

[

Amazon.co.jpで購入する

](https://www.amazon.co.jp/dp/4798193429?tag=note0e2a-22&linkCode=ogi&th=1&psc=1&language=ja_JP)

[#AI](https://note.com/hashtag/AI) [#生成AI](https://note.com/hashtag/%E7%94%9F%E6%88%90AI) [#AIエージェント](https://note.com/hashtag/AI%E3%82%A8%E3%83%BC%E3%82%B8%E3%82%A7%E3%83%B3%E3%83%88) [#AI時代](https://note.com/hashtag/AI%E6%99%82%E4%BB%A3) [#AI活用](https://note.com/hashtag/AI%E6%B4%BB%E7%94%A8) [#AI人材](https://note.com/hashtag/AI%E4%BA%BA%E6%9D%90) [#AI研修](https://note.com/hashtag/AI%E7%A0%94%E4%BF%AE) [#AIツール](https://note.com/hashtag/AI%E3%83%84%E3%83%BC%E3%83%AB) [#Claude](https://note.com/hashtag/Claude) [#ClaudeCode](https://note.com/hashtag/ClaudeCode)

600本以上の記事が読み放題。AI × デザインを主軸に、独自の視点やノウハウをお届けします。AI時…[このメンバーシップの詳細](https://note.com/kawaidesign/membership/join)

610名が参加中