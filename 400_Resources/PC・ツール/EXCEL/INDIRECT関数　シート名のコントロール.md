---
title: "INDIRECT関数　シート名のコントロール"
created: 2026-05-02
tags: []
---

#excel
[https://excelcamp.jp/media/function/26813/](https://excelcamp.jp/media/function/26813/)



シート名を"月次決算"として、
A1のセルに、月次決算　と入れておく。

=INDIRECT($A1&"!N1"))
の式の値は、シート「月次決算」のN1の値　になる
