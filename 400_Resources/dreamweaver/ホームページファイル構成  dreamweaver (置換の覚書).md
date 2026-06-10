---
title: "ホームページファイル構成  dreamweaver (置換の覚書)"
created: 2026-05-05
tags: []
---

●00web_working

ローカル：　\Dropbox\web\working_web\

リモート：　\それぞれのPC○○\WEB\web\

最初に作成するファイル

●01web

ローカル：　\それぞれのPC○○\WEB\web\

リモート：　\それぞれのPC○○\WEB\webup\

Ctrl+Shift+Qもしくは、

修正　＞　テンプレート　＞　マークアップを省略して書き出し

フォルダ：file:///C|/Users/osamu/WEB/webup/

![[Pasted image 20260505011047.png]]

●02web_up

ローカル：\それぞれのPC○○\WEB\webup\

リモート：/home/sub00944/www/mft.jp/

**FTPホスト：140.○○.125.25  (×http:140.○○.125.25)**　番号は例

１．検索置換で

<!-- #(Begin|End)LibraryItem.*? -->

検索(E): ソースコード

オプション: 正規表現を使用にチェックを入れる

空白の一斉置換でライブラリーのマークアップを消去

**「検索対象」は変更するファイル数の多さによって随時選択する**

![[Pasted image 20260505011139.png]]

●02web_up  ※現在は利用していない

ローカル：\それぞれのPC○○\WEB\webup\

リモート：/home/sub00944/www/mft.jp/

**FTPホスト：140.○○.125.25  (×http:140.○○.125.25)**　番号は例

１．検索置換で

<!-- #(Begin|End)LibraryItem.*? -->

検索(E): ソースコード

オプション: 正規表現を使用にチェックを入れる

空白の一斉置換でライブラリーのマークアップを消去

**「検索対象」は変更するファイル数の多さによって随時選択する**


![[Pasted image 20260505011401.png]]


●05sp_up

ローカル： \それぞれのPC○○\WEB\sp_up\

リモート：/home/sub00944/www/mft.jp/sp/

１．検索置換で

<!-- #(Begin|End)LibraryItem.*? -->

空白の一斉置換でライブラリーのマークアップを消去

![[Pasted image 20260505011139.png]]


２．gzip圧縮してサーバーにアップロードする

大量ファイルはgzComp　もしくは数ファイルの場合は、Lhaplusを利用する

---

置換の覚書

[http://tasudesign.com/web-desing/dw-sr/](http://tasudesign.com/web-desing/dw-sr/)

- _｢検索｣・・・`<td>りんご(.*)個</td>`_
- _｢置換｣・・・`<td>メロン$1玉</td>`_

_【(._)】は正規表現(文字列のグループ化)でそのまま【$1】へ中の文字列を挿入してくれます。*(これで中の数字が違っても大丈夫)

※`<td></td>`は省いても大丈夫ですが、今回はわかりやすいように記述しています。

### 手順3


![[Pasted image 20260505011626.png]]