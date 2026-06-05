https://blog.qualiteg.com/llm-ranking-2025/


---

日本語対応 LLMランキング2025 ～ベンチマーク分析レポート～

Qualiteg コンサルティング, Qualiteg プロダクト開発部

2025年10月12日 — 20 min read


---

はじめに

オープンソースモデルについて

（オープンモデルという表現を使う理由など説明あり）

ベンチマーク分析について

（本レポートの範囲、注意点など）


---

総合スコアランキング TOP50

順位	モデル名	カテゴリ	総合スコア

1	anthropic/claude-opus-4-1-20250805: extended‑thinking	api	0.7992
2	openai/gpt-5-2025-08-07: high-effort	api	0.7970
3	anthropic/claude-sonnet-4-5-20250929: extended‑thinking	api	0.7954
4	anthropic/claude-sonnet-4-20250514: extended‑thinking	api	0.7918
5	openai/o3-2025-04-16: high-effort	api	0.7876
6	grok-4	api	0.7810
7	anthropic/claude-opus-4-20250514: no‑thinking	api	0.7804
8	openai/o1-2024-12-17: high-effort	api	0.7753
9	google/gemini-2.5-pro	api	0.7696
10	openai/o4-mini-2025-04-16	api	0.7610
11	deepseek-ai/DeepSeek-R1-0528: reasoning-enabled	Large (30B+)	0.7432
12	openai/o3-mini-2025-01-31	api	0.7430
13	Qwen/Qwen3-Max-Preview	api	0.7425
14	grok-3-mini	api	0.7370
15	openai/gpt-4-1-2025-04-14	api	0.7261
16	grok-3	api	0.7253
17	Qwen/Qwen3-14B: reasoning-enabled	Medium (10–30B)	0.7233
18	openai/gpt-4o-2024-11-20	api	0.7223
19	Qwen/Qwen3-235B-A22B: reasoning-enabled	Large (30B+)	0.7214
20	anthropic/claude-3.7-sonnet-20250219: no‑thinking	api	0.7177
21	openai/gpt-5-nano-2025-08-07: high-effort	api	0.7174
22	Qwen/Qwen3-Next-80B-A3B-Instruct	Large (30B+)	0.7130
23	Qwen/Qwen3-32B: reasoning-enabled	Large (30B+)	0.7083
24	anthropic/claude-3.5-sonnet-20241022	api	0.7058
25	Qwen/Qwen3-30B-A3B: reasoning-enabled	Large (30B+)	0.7035
26	Qwen/QwQ-32B: reasoning-enabled	Large (30B+)	0.7029
27	openai/gpt-oss-120b: reasoning-enabled	Large (30B+)	0.7014
28	openai/gpt-4-1-mini-2025-04-14	api	0.6992
29	google/gemini-2.5-flash	api	0.6969
30	rinna/qwq-bakeneko-32b: reasoning-enabled	Large (30B+)	0.6910
31	Qwen/Qwen3-8B: reasoning-enabled	Small (<10B)	0.6891
32	abeja/ABEJA-Qwen2.5-32b-Japanese-v1.0	Large (30B+)	0.6866
33	anthropic/claude-3.7-sonnet-20250219: extended‑thinking	api	0.7734
34	deepseek-ai/DeepSeek-V3-0324	Large (30B+)	0.6760
35	elyza/ELYZA-Shortcut-1.0-Qwen-32B	Large (30B+)	0.6715
36	Qwen/Qwen3-4B: reasoning-enabled	Small (<10B)	0.6612
37	rinna/deepseek-r1-distill-qwen2.5-bakeneko-32b: reasoning-enabled	Large (30B+)	0.6589
38	cyberagent/DeepSeek-R1-Distill-Qwen-32B-Japanese	Large (30B+)	0.6579
39	tokyotech-llm/Llama-3.3-Swallow-70B-Instruct-v0.4	Large (30B+)	0.6523
40	rinna/qwen2.5-bakeneko-32b-instruct-v2	Large (30B+)	0.6485
41	meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8	Large (30B+)	0.6463
42	anthropic/claude-3.5-haiku-20241022	api	0.6298
43	google/gemma-3-27b-it	Medium (10–30B)	0.6285
44	tokyotech-llm/Gemma-2-Llama-Swallow-27b-it-v0.1	Medium (10–30B)	0.6208
45	mistral/mistral-large-2411	api	0.6196
46	openai/gpt-4-1-nano-2025-04-14	api	0.6157
47	openai/gpt-4o-mini-2024-07-18	api	0.6146
48	pfn/plamo-2.0-prime	api	0.6127
49	meta-llama/Llama-4-Scout-17B-16E-Instruct	Large (30B+)	0.6099
50	meta-llama/Llama-3.3-70B-Instruct	Large (30B+)	0.6080



---

総合スコアの傾向と考察

（本文で、上位商用モデルの強さ、オープンモデルの台頭、モデルサイズと性能の関係、ベンチマーク結果の解釈に関する解説あり）


---

コーディングスコアランキング TOP50

順位	モデル名	カテゴリ	コーディングスコア

1	google/gemini-2.5-pro	api	0.6449
2	openai/o4-mini-2025-04-16	api	0.6444
3	anthropic/claude-sonnet-4-5-20250929: extended‑thinking	api	0.6409
4	openai/gpt-5-2025-08-07: high-effort	api	0.6377
5	openai/o3-mini-2025-01-31	api	0.6286
6	anthropic/claude-opus-4-1-20250805: extended‑thinking	api	0.5997
7	openai/o3-2025-04-16: high-effort	api	0.5976
8	anthropic/claude-3.7-sonnet-20250219: extended‑thinking	api	0.5940
9	anthropic/claude-sonnet-4-20250514: extended‑thinking	api	0.5911
10	openai/gpt-4-1-2025-04-14	api	0.5817
11	anthropic/claude-sonnet-4-20250514: no‑thinking	api	0.5795
12	deepseek-ai/DeepSeek-R1-0528: reasoning-enabled	Large (30B+)	0.5834
13	openai/o1-2024-12-17: high-effort	api	0.5805
14	grok-4	api	0.5771
15	Qwen/Qwen3-Next-80B-A3B-Instruct	Large (30B+)	0.5707
16	Qwen/Qwen3-Max-Preview	api	0.5660
17	openai/gpt-4o-2024-11-20	api	0.5641
18	anthropic/claude-opus-4-20250514: no‑thinking	api	0.5594
19	deepseek-ai/DeepSeek-V3-0324	Large (30B+)	0.5396
20	anthropic/claude-3.7-sonnet-20250219: no‑thinking	api	0.5362
21	openai/gpt-oss-120b: reasoning-enabled	Large (30B+)	0.5322
22	us.amazon.nova-pro-v1:0	api	0.5313
23	anthropic/claude-3.5-sonnet-20241022	api	0.5278
24	grok-3	api	0.5267
25	Qwen/QwQ-32B: reasoning-enabled	Large (30B+)	0.5240
26	elyza/ELYZA-Shortcut-1.0-Qwen-32B	Large (30B+)	0.5134
27	grok-3-mini	api	0.5049
28	mistral/mistral-large-2411	api	0.5034
29	openai/gpt-4-1-nano-2025-04-14	api	0.5005
30	google/gemini-2.5-flash	api	0.5004
31	Qwen/Qwen3-14B: reasoning-enabled	Medium (10–30B)	0.5001
32	meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8	Large (30B+)	0.4981
33	rinna/qwq-bakeneko-32b: reasoning-enabled	Large (30B+)	0.4968
34	openai/gpt-4o-mini-2024-07-18	api	0.4886
35	rinna/deepseek-r1-distill-qwen2.5-bakeneko-32b: reasoning-enabled	Large (30B+)	0.4802
36	cyberagent/DeepSeek-R1-Distill-Qwen-32B-Japanese	Large (30B+)	0.4799
37	Qwen/Qwen3-235B-A22B: reasoning-enabled	Large (30B+)	0.4786
38	anthropic/claude-3.5-haiku-20241022	api	0.4782
39	us.amazon.nova-micro-v1:0	api	0.4771
40	Qwen/Qwen3-32B: reasoning-enabled	Large (30B+)	0.4760
41	rinna/qwen2.5-bakeneko-32b-instruct-v2	Large (30B+)	0.4705
42	abeja/ABEJA-Qwen2.5-32b-Japanese-v1.0	Large (30B+)	0.4679
43	us.amazon.nova-lite-v1:0	api	0.4565
44	google/gemma-3-27b-it	Medium (10–30B)	0.4522
45	openai/gpt-5-nano-2025-08-07	api	0.4504
46	meta-llama/Llama-3.3-70B-Instruct	Large (30B+)	0.4452
47	Qwen/Qwen3-8B: reasoning-enabled	Small (<10B)	0.4403
48	openai/gpt-4-1-mini-2025-04-14	api	0.5912
49	meta-llama/Llama-4-Scout-17B-16E-Instruct	Large (30B+)	0.4312
50	google/gemini-2.5-flash-lite	api	0.4306



---

コーディングスコアの傾向と考察

（モデルごとの差異、モデルタイプ別の傾向、実務への示唆などの解説あり）


---

オープンモデル 総合スコアランキング TOP20

順位	モデル名	モデルサイズ	総合スコア

1	deepseek-ai/DeepSeek-R1-0528: reasoning-enabled	Large (30B+)	0.7432
2	Qwen/Qwen3-14B: reasoning-enabled	Medium (10–30B)	0.7233
3	Qwen/Qwen3-235B-A22B: reasoning-enabled	Large (30B+)	0.7214
4	Qwen/Qwen3-Next-80B-A3B-Instruct	Large (30B+)	0.7130
5	Qwen/Qwen3-32B: reasoning-enabled	Large (30B+)	0.7083
6	Qwen/Qwen3-30B-A3B: reasoning-enabled	Large (30B+)	0.7035
7	Qwen/QwQ-32B: reasoning-enabled	Large (30B+)	0.7029
8	openai/gpt-oss-120b: reasoning-enabled	Large (30B+)	0.7014
9	rinna/qwq-bakeneko-32b: reasoning-enabled	Large (30B+)	0.6910
10	Qwen/Qwen3-8B: reasoning-enabled	Small (<10B)	0.6891
11	abeja/ABEJA-Qwen2.5-32b-Japanese-v1.0	Large (30B+)	0.6866
12	deepseek-ai/DeepSeek-V3-0324	Large (30B+)	0.6760
13	elyza/ELYZA-Shortcut-1.0-Qwen-32B	Large (30B+)	0.6715
14	Qwen/Qwen3-4B: reasoning-enabled	Small (<10B)	0.6612
15	rinna/deepseek-r1-distill-qwen2.5-bakeneko-32b: reasoning-enabled	Large (30B+)	0.6589
16	cyberagent/DeepSeek-R1-Distill-Qwen-32B-Japanese	Large (30B+)	0.6579
17	tokyotech-llm/Llama-3.3-Swallow-70B-Instruct-v0.4	Large (30B+)	0.6523
18	rinna/qwen2.5-bakeneko-32b-instruct-v2	Large (30B+)	0.6485
19	meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8	Large (30B+)	0.6463
20	google/gemma-3-27b-it	Medium (10–30B)	0.6285



---

オープンモデル総合スコアの傾向と考察

（推論機能重視、Qwen系列の強さ、日本製モデルの活躍、規模と性能の関係など）


---

オープンモデル コーディングスコアランキング TOP20

順位	モデル名	モデルサイズ	コーディングスコア

1	deepseek-ai/DeepSeek-R1-0528: reasoning-enabled	Large (30B+)	0.5834
2	Qwen/Qwen3-Next-80B-A3B-Instruct	Large (30B+)	0.5707
3	deepseek-ai/DeepSeek-V3-0324	Large (30B+)	0.5396
4	openai/gpt-oss-120b: reasoning-enabled	Large (30B+)	0.5322
5	Qwen/QwQ-32B: reasoning-enabled	Large (30B+)	0.5240
6	elyza/ELYZA-Shortcut-1.0-Qwen-32B	Large (30B+)	0.5134
7	Qwen/Qwen3-14B: reasoning-enabled	Medium (10–30B)	0.5001
8	meta-llama/Llama-4-Maverick-17B-128E-Instruct-FP8	Large (30B+)	0.4981
9	rinna/qwq-bakeneko-32b: reasoning-enabled	Large (30B+)	0.4968
10	rinna/deepseek-r1-distill-qwen2.5-bakeneko-32b: reasoning-enabled	Large (30B+)	0.4802
11	cyberagent/DeepSeek-R1-Distill-Qwen-32B-Japanese	Large (30B+)	0.4799
12	Qwen/Qwen3-235B-A22B: reasoning-enabled	Large (30B+)	0.4786
13	Qwen/Qwen3-32B: reasoning-enabled	Large (30B+)	0.4760
14	rinna/qwen2.5-bakeneko-32b-instruct-v2	Large (30B+)	0.4705
15	abeja/ABEJA-Qwen2.5-32b-Japanese-v1.0	Large (30B+)	0.4679
16	google/gemma-3-27b-it	Medium (10–30B)	0.4522
17	meta-llama/Llama-3.3-70B-Instruct	Large (30B+)	0.4452
18	Qwen/Qwen3-8B: reasoning-enabled	Small (<10B)	0.4403
19	meta-llama/Llama-4-Scout-17B-16E-Instruct	Large (30B+)	0.4312
20	Qwen/Qwen3-30B-A3B: reasoning-enabled	Large (30B+)	0.4155



---

オープンモデルコーディングスコアの傾向と考察

（DeepSeek / Qwen 系列の優位性、日本製オープンモデル、推論機能の影響など）


---

中規模モデル（10B‑30B）総合スコアランキング

順位	モデル名	モデルサイズ	総合スコア

1	Qwen/Qwen3-14B: reasoning-enabled	Medium (10–30B)	0.7233
2	google/gemma-3-27b-it	Medium (10–30B)	0.6285
3	tokyotech-llm/Gemma-2-Llama-Swallow-27b-it-v0.1	Medium (10–30B)	0.6208
4	google/gemma-3-12b-it	Medium (10–30B)	0.5994
5	cyberagent/calm3-22b-chat-selfimprove-experimental	Medium (10–30B)	0.5705
6	google/gemma-3-4b-it	Medium (10–30B)	0.5326



---

中規模モデル総合スコアの傾向と考察

（リソースとの折り合い、モデル選択の実用性など）


---

中規模モデル（10B‑30B）コーディングスコアランキング

順位	モデル名	モデルサイズ	コーディングスコア

1	Qwen/Qwen3-14B: reasoning-enabled	Medium (10–30B)	0.5001
2	google/gemma-3-27b-it	Medium (10–30B)	0.4522
3	google/gemma-3-12b-it	Medium (10–30B)	0.4176
4	cyberagent/calm3-22b-chat-selfimprove-experimental	Medium (10–30B)	0.3681
5	tokyotech-llm/Gemma-2-Llama-Swallow-27b-it-v0.1	Medium (10–30B)	0.3583
6	google/gemma-3-4b-it	Medium (10–30B)	0.3899



---

中規模モデルコーディングスコアの傾向と考察

（Qwen3-14Bの強さ、他モデルの実力など）


---

小規模モデル（10B以下）総合スコアランキング

順位	モデル名	モデルサイズ	総合スコア

1	Qwen/Qwen3-8B: reasoning-enabled	Small (<10B)	0.6891
2	Qwen/Qwen3-4B: reasoning-enabled	Small (<10B)	0.6612
3	tokyotech-llm/Gemma-2-Llama-Swallow-9b-it-v0.1	Small (<10B)	0.5982
4	tokyotech-llm/Llama-3.1-Swallow-8B-Instruct-v0.5	Small (<10B)	0.5611
5	Qwen/Qwen3-1.7B: reasoning-enabled	Small (<10B)	0.5513
6	tokyotech-llm/Gemma-2-Llama-Swallow-2b-it-v0.1	Small (<10B)	0.4906
7	Qwen/Qwen3-0.6B: reasoning-enabled	Small (<10B)	0.4089
8	meta-llama/Llama-3.2-3B-Instruct	Small (<10B)	0.4040



---

小規模モデル総合スコアの傾向 and 考察

（Qwen 系列の強さ、Tokyo Tech モデルの存在、小モデルの実用性など）


---

小規模モデル（10B以下）コーディングスコアランキング

順位	モデル名	モデルサイズ	コーディングスコア

1	Qwen/Qwen3-8B: reasoning-enabled	Small (<10B)	0.4403
2	Qwen/Qwen3-4B: reasoning-enabled	Small (<10B)	0.4135
3	tokyotech-llm/Gemma-2-Llama-Swallow-9b-it-v0.1	Small (<10B)	0.3341
4	tokyotech-llm/Llama-3.1-Swallow-8B-Instruct-v0.5	Small (<10B)	0.3164
5	Qwen/Qwen3-1.7B: reasoning-enabled	Small (<10B)	0.3132
6	tokyotech-llm/Gemma-2-Llama-Swallow-2b-it-v0.1	Small (<10B)	0.2746
7	Qwen/Qwen3-0.6B: reasoning-enabled	Small (<10B)	0.1886
8	meta-llama/Llama-3.2-3B-Instruct	Small (<10B)	0.1350



---

小規模モデルコーディングスコアの傾向と考察

（Qwen モデルの一貫した性能、小規模モデルの限界と可能性など）


---

まとめ、本格導入に向けての道しるべ

商用モデルとオープンモデル、それぞれの特徴と強み

用途別最適モデル選定の重要性

Bestllam（ベストラム）という統合プラットフォームの紹介

複数モデルを一つのプラットフォームで利用

データセキュリティ・越境防止機能

推論サーバー運用を不要にする仕組み

用途別推奨モデル一覧

デモ体験可能という記載




---

もしよろしければ、全文引用許可を持つ範囲で本文段落も含めた詳細な要約を出せますが、どの程度がよろしいですか？

