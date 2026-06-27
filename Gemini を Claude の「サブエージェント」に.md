Google Cloud Japan
Google Cloud Japan



目次
Gemini を Claude の「サブエージェント」に —— 大規模開発でコストを実測

Yuting Lin
2026/06/25に公開

Google Cloud

Vertex AI

GenAI

Claude Code

Google Antigravity

tech
Claude Code が指揮し、Gemini（Antigravity CLI ）が実行する構成図

!
Google Cloud Japan シリーズ続編です。

Claude on Vertex AI 完全ガイド（導入）
Claude × Gemini マルチモデルゲートウェイを Cloud Run で構築（ランタイムのコスト追跡）
本記事 ← 開発時(dev-time) のハイブリッド構成。実測して効果と効きどころを確かめました。
TL;DR
Claude Code が指揮し、Gemini（Antigravity CLI agy）が実行するハイブリッド構成を、Claude Code Plugin Antigravity for Claude Code として実装し、実測しました。

大規模な実作業で Claude コストを 27〜64% 削減——しかも同品質（同一 adk eval ゲート通過）。重い処理は Gemini 側に逃がすので、実際の総コスト差はさらに大きい。
コスト以外の価値も大きい: 社内データ検索（Vertex AI Search）、Web/deep research、クロスモデル検証、ADK 純正のマルチエージェント構築→Agent Engine デプロイ——いずれも Claude 単独では難しく、規模に依らず常に効く。
効きどころは実のある仕事（大規模機能・一括移行・網羅テスト・多エージェント・大量検索）。小さな単発は Claude 直で十分＝適材適所で使うのがコツ。
結論を先に言うと、「判断は優秀なモデル、量産はコスパの良いモデル」の振り分けは、まとまった仕事ほど効きます。 以下、構成・実測・使いこなしをまとめます。

!
用語ミニ解説

Claude Code … Anthropic の Claude で動くコーディング CLI（ターミナルの AI 開発エージェント）。
Antigravity CLI（agy） … Google の agent-first な CLI（Gemini で動く）。
Plugin … Claude Code を拡張し、チームへ配布・バージョン管理できる仕組み（公式）。個人・単一プロジェクトなら標準設定、共有・配布したいなら Plugin、という住み分け。
Skill / Hook / Subagent … Plugin の構成要素。Skill＝Claude への手順書（公式）、Hook＝起動時などに自動で走る処理（公式）、Subagent＝役割を絞った子エージェント（公式）。
背景：なぜ今、これを作ったのか
タイミング：agy が Google の "agent-first" CLI の本命になった。 2026年5月の Google I/O で、Gemini CLI は Antigravity CLI（agy）に統合されました（Go 製・Node 不要、Antigravity 2.0 と同じエージェントハーネス）。6/18 には Gemini CLI が個人向け（AI Pro/Ultra・無料 Code Assist）で提供終了（Enterprise / Google Cloud 経由は継続）。しかも agy は Agent Skills・Hooks・Subagents・Plugins を一級機能として備えます。「Gemini を Claude のサブエージェントに」という構想を、ちょうど現実的に組める土台が整ったわけです。

動機：AI コーディング支出は、いまや経営課題。
AI コーディング支出高すぎ問題直近のニュースが象徴的です。Uber は Claude Code を約5,000人のエンジニアに展開し採用率は84〜95%に達したものの、2026年のAIコーディング予算をわずか4ヶ月で使い切りました（エンジニア1人あたりの API 費は月 $500〜2,000 という報道も）。COO は「ユーザーに届く価値へ直線が引けないと、その投資は正当化しにくい」と述べています（Fortune）。Microsoft も社内の Claude Code ライセンスの多くを停止し自社 CLI へ寄せると報じられ（TheStreet）、Nadella は「有用な成果を出せなければ"トークンを生成する社会的許可"すら失う」と警告しています（TechRadar）。

構造的な理由はシンプルで、トークン課金のツールは「便利なほど高くつく」——シートライセンスと違い、使うほど請求が伸びます。技術的にもコストの主因は出力ではなく入力で、エージェントは毎ターン文脈を読み直す "context snowball" により後半ほど高くつき、トークンを増やせば性能が上がるわけでもありません（Anthropic はエージェントで約4倍・マルチで約15倍、Stanford は入力主導でトークン増≠性能と報告）。だから問いは「AI を使うか否か」ではなく「1トークンあたりの価値をどう最大化するか」になりました。

効くレバーは明快。 上の数字（入力コスト・context snowball）が示すのは、コンテキストを薄く保ち、量産を別のモデルへ逃がす——すなわち「判断は Claude、実行は Gemini」という階層化が効く、ということです。本 Plugin はこの仕組みを固定し、効果を実測で裏取りしました（後述の −27〜64%、しかも同品質）。

そこで私が考えたのが、「Claude Code で Antigravity CLI を使う」というソリューションです。主張だけでなく、実測の裏付けとすぐ使える成果物として提供したかった——それがこの Plugin です。

コンセプト：Gemini を Claude のサブエージェントに
開発時のエージェントを2層に分けます。



指揮層 (Claude Code)：要件・設計・難所(hard 20%)・検証・レビュー。判断が要る所。
実行層 (agy / Gemini)：scaffold・定型実装・テスト生成・一次レビュー・Web/社内検索など、決定論的で量の多い作業。
両者が同じ Vertex/GCP 課金に乗るので、判断は Claude に、量産はそのタスクでコスパの良い Gemini に振り分ける（"intelligent model routing"）が成立します。実装は agy --print をヘッドレスで叩く薄いラッパー＋「いつ・どう委譲し・どう検証するか」を定義した Skill が核。さらに次の2つを同梱し、スキルを明示的に呼ばなくても規律が効くようにしています。

委譲専用サブエージェント：使えるツールを委譲ラッパーだけに制限（Write/Edit/自由な Bash を持たない）。ファイル生成は Gemini 側で完結し、Claude はファイル内容の生成にトークンを使いません。
セッション開始フック：起動時にコスト方針（分岐点の上で委譲・コンテキストを薄く・必ず検証）をコンテキストへ自動注入。ON/OFF は Plugin 設定で切替可。
!
agy は複数モデル対応です。本記事は Gemini を実行役にした構成ですが、--model や Plugin 設定（default_model / tier_*）で他モデルにも切替できます。ただし指揮役 Claude とは別系統の安いモデルを実行役にするのが、コスト削減とクロスモデル検証の前提です（Claude が Claude を実行するとどちらの利点も消えます）。

# 指揮役 Claude から、定型作業を agy(Gemini) に委譲する一行
"$CLAUDE_PLUGIN_ROOT/scripts/agy-delegate.sh" --tier flash --dir . \
  "このリポの全 README を3行で要約。要点だけ返す（生本文は貼らない）"

構成：Plugin の全体像
Claude Code 側に「指揮の部品」を置き、その下で Antigravity CLI を呼ぶ「実行の部品」につなぐ——という薄い構成です。

Pluginの構成図

skills/SKILL.md … いつ・どう委譲し・どう検証するか（Plugin の頭脳）
commands/ … Claude Code 上で使用可能なスラッシュコマンド（delegate / review / research / setup / status / result / cancel）
hooks/ … セッション開始時に agy をチェック＋コスト方針を自動注入（サブエージェントの Bash 制限も）
agents/antigravity-delegate … ツールを委譲ラッパーに限定した委譲専用サブエージェント
scripts/ … agy-delegate（ラッパー）／agy-job（背景ジョブ）／measure-session（計測）／doctor
.claude-plugin/ … plugin.json（userConfig）・marketplace.json
各部品の仕様・config の設定方法・データフローは、こちらの技術リファレンスで詳しく解説します。


なぜ Skill ではなく Plugin にしたか
agy も Claude Code も Skill・Hook・Subagent・Plugin を一級機能として持ちます。だから「Skill だけで十分では？」は正当な問いで、公開後にも実際に聞かれました。

実際、Plugin といっても頭脳は Skill です。本 Plugin の頭脳は SKILL.md（いつ・どう委譲し・どう検証するか）で、手書きの Skill でも価値の8割は出ます。それでも Plugin として畳んだのは、Skill 単体では足りない／再現しにくい部分があるからです。OpenAI も Codex の Claude Code Plugin を出しています。

核心は 確実性 です。Skill は LLM が解釈する指示書なので、守られるかどうかに揺れ（挙動の不確定性）が出ます。一方 Plugin は実際のコード（ラッパー・フック・権限制限のスクリプト）を同梱するので、要所は LLM の解釈に頼らず決定的に動く。つまり 判断は Skill（柔らかく賢く）、確実性はコード（固く確実に） の二段構え——以下はその具体例です。

agy のヘッドレス特有の落とし穴を内蔵：-p はプロンプトを"値"に取り最後に置く／非TTYで stdout が落ちるので < /dev/null で外す／タイムアウト／ツール使用は --yolo。これらを毎回"再発見"せずに済みます（「agy が止まったまま戻らない」という報告は、まさにこの stdin 由来）。
Skill 単体では出来ないこと：バックグラウンドジョブ、構造化エラー（quota／認証／timeout を終了コードで区別）、セッション開始フックでの方針自動注入、そして ツールを委譲ラッパーに限定したサブエージェント（ファイル生成のトークンを Claude 側で使わない）。
共有可能性・再現性：テスト・CI・doctor・ワンコマンド導入・claude plugin validate 通過。Skill を自作して育て続けない人でも、同じ規律で確実に動きます。
正直に言うと、すでに自作 Skill を運用・改善している人は、上記の多くは要りません。Plugin は「導入してすぐ＋追加機能＋再現性」が欲しい人向け、という住み分けです。

実測：大規模ビルドでの効果
題材は、Google ADK で SDLC アシスタントのマルチエージェント（要件定義→基本設計→詳細設計を SequentialAgent で連結、各エージェントが Web 検索でグラウンディング）を構築し、adk eval を通すまで。同一モデル・同一タスク・3アームで比較しました。



指標	Claude 単独@high	単独@max	ハイブリッド
output	123,216	388,676	113,351
cache_read	10.19M	20.45M	5.65M
turns	126	154	87
COST-WEIGHTED（相対指標・低いほど安い）	2.62M	5.34M	1.91M
実額（Claude側・概算USD）	$13.1	$26.7	$9.6
adk eval	✅3/3	✅3/3	✅3/3
!
COST-WEIGHTED は実トークン数でもドルでもなく、単価で重み付けした相対コスト指標です（内訳：output×5 + input×1 + cache_create×1.25 + cache_read×0.1 ＝ Claude Opus の標準倍率）。トークン総数でなく実コストに近い形で比較するためのものです。「実額」行は Opus の入力単価で概算した Claude 側のみの値で、Gemini 側は別計上（いずれも概算・実測 n=1）。

ハイブリッドは単独@high より −27%、単独@max より −64%、しかも同品質。
ターン最少（87 vs 126 vs 154）→ cache_read 半減、output も最少。重い実装をGemini 側に逃がした結果（その分は未計上＝実差はさらに大）。
「最強の単独（@max）を投げる」は同品質に対して最も高コストでした。
全トークン内訳と再現方法（n=1・Claude側）
効きどころ（適材適所）
効果は規模に依存します。まとまった仕事——大規模機能・一括移行・網羅テスト・多エージェント構築・大量ドキュメント横断——で最大の ROI が出ます。

逆に、小さな単発タスクは委譲の往復コストが勝つので、Claude Code だけで十分。無理に委譲しないのが正解です。

!
参考: 小さな天気アプリ程度だとハイブリッドは単独よりやや高くつきました。
つまり「全部ハイブリッド」ではなく、実のある仕事に効かせる——これが使いこなしの第一歩です。

コスト以外の価値（規模に依らず常に効く）
社内データ検索 (Vertex AI Search): 自社の PRD・設計標準などに grounding。Claude 単独にはない能力。
Web / deep research: 最新の事実を根拠付きで取得し、Claude が再確認。/antigravity:research コマンドで、agy が安価に検索の足を稼ぎ→Claude が2ソース以上で裏取りして合成する“検証必須”のリサーチを定型化（agy の print モードの引用は粗いので、生の出典は鵜呑みにしない設計）。
クロスモデル検証: 別モデル（Gemini）が独立レビュー → 二重チェックで品質と信頼性。
Google 純正フロー: ADK でマルチエージェントを建て、Agent Engine にデプロイまで一気通貫。
前作のマルチモデルゲートウェイがランタイムのコストを LiteLLM+BigQuery+Looker で可視化したのに対し、本構成は開発時のコストを可視化。組み合わせると開発〜運用のコストが一気通貫で見えます。

使いこなし（効果を最大化する4つの勘所）
Plugin の Skill に "Cost discipline" として明文化しています。

分岐点の上で委譲：まとまった量のある仕事に使う。
Claude のコンテキストを薄く保つ：agy の生成物は読み戻さず、要約だけ受け取る（cache_read を最小化する最大レバー）。
単発バッチ：往復を減らす。
diff だけレビュー：全文でなく差分で。
安心して使える設計
検証ゲート：Claude が必ず実行して確認し、自己申告を鵜呑みにしません。実際の検証中、実行側が adk eval を通すために環境を改ざんした不正を検知して是正しました——「Claude が検証する」価値が効いた一幕です。
運用上の注意も明文化：ヘッドレス（claude -p）では委譲は同期で行う／Agent Engine デプロイは SDK 経路＋明示 requirements（google-adk[a2a]）で、など実測で踏んだ落とし穴を Skill に反映。
構造化エラー：quota / 認証 / タイムアウト / 未インストールを終了コードで区別し、バックグラウンドジョブが状態を表面化（quota なら --continue での再開を提示）。失敗を握りつぶさず、上位が機械的に対処できます。
公開後の実フィードバックを反映：例として WSL の /mnt マウントは agy の --add-dir が 9p 越しで極端に遅くなるため、ラッパーと doctor が警告を出すようにしました（修正＝リポを Linux FS 側へ）。実利用の声を取り込んで改善を継続中。
テスト・CI・doctor 同梱、公式 claude plugin validate 通過。
始め方
Plugin 画面

/plugin marketplace add yuting0624/antigravity-for-claude-code
/plugin install antigravity@antigravity-for-claude-code
/antigravity:setup   # まず環境チェック（doctor）



インストール後はセッション開始時にコスト方針が自動で入るので、まず使い始めるだけでOK。既定 tier・タイムアウト・方針注入の ON/OFF は Plugin 設定（userConfig）で調整できます。

スラッシュコマンド
コマンド	何をする
/antigravity:setup	環境チェック（agy の導入・認証、doctor）
/antigravity:delegate [--tier flash|pro] <task>	タスクを agy(Gemini) に委譲 → Claude が検証
/antigravity:review [--adversarial]	現在の diff を Gemini が独立レビュー → Claude が統合
/antigravity:research <topic>	Claude 指揮の deep research（agy が検索 → Claude が2ソース裏取り）
/antigravity:status [id] ／ :result <id> ／ :cancel <id>	背景ジョブの一覧・結果取得・中止（対話セッション向け）
背景ジョブ（status / result / cancel）は対話セッション用。ヘッドレス（claude -p）では委譲を同期で実行します。

おわりに
「判断は優秀なモデル、量産はコスパの良いモデル」という振り分けは、実のある開発でコストを実測 27〜64% 下げつつ（同品質）、Claude 単独にはない能力まで足せる——というのが今回の手応えです。生成はもう解決済み。次に効くのは検証と方向付けで、それを2つの AI で回すのが現実的な最適解だと思います。

Gemini x Claude コラボ

公開後は Reddit で 27K views・84 upvotes・48 comments と海外の開発者からも反応をもらい、実フィードバック（WSL でのI/O遅延、ヘッドレスでの落とし穴など）を取り込んで改善を続けています。MIT のコミュニティプロジェクトなので、Issue・PR・⭐ いずれも歓迎です。

補足：明示型ルーティングの先にあるもの
本記事は「人間が振り分けを設計する明示型ルーティング」を、実測で裏取りした話でした。指揮も検証もこちらが握れる——透明で監査可能な構成です。

一方で2026年6月、Sakana AI の Fugu のように「モデル自身が振り分けを学習する」オーケストレーションも登場しました。複数の他社フロンティアモデルを単一 LLM として束ね、選定・委譲・検証・統合を内部で自動化する——強力な一方、どのモデルをどう呼んだかは proprietary（非公開）です。

「賢く自動化された黒箱」と「透明で検証可能な白箱」、エンタープライズはどちらを採るべきか。コスト可視性・データ経路・監査といったところが論点になりそうです。

免責: コミュニティ検証であり Google / Anthropic 非公式です。トークン/クラウド費用・認証・データ共有は自己責任で。数値は実測（n=1）の実例です。