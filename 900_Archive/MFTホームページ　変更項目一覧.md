---
title: "MFTホームページ　変更項目一覧"
created: 2026-05-02
tags: []
---

#0終了後削除 

ctrl+alt+Q
	index.dwtを適用させる
	テンプレートにチェックを入れる
face rightwaku　を削除してから

---

	<ul class="pagelinks" style="width: 128px;">
	　　　　　　　　↓
	<ul class="pagelinks" style="width: 250px;">

---

	<link rel="alternate" media="only screen and [\s\S]*?">
	　　　　　　　　↓
	null
---
	<!-- #BeginLibraryItem "/Library/navi_head2.lbi" -->
	　　　　　　　　↓
	<!-- #BeginLibraryItem "/Library/navi_head.lbi" -->

---
s_navisideにnavisideを反映させて、ページ下に、表示させる

---

CSS tableの修正をする