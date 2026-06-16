-  VBA作動中に画面の遷移を止めちらつき防止、最後に戻す
```
Application.ScreenUpdating = False
```
```
Application.ScreenUpdating = True
```

- **アラートを止める**
```
Application.DisplayAlerts = False
```
```
Application.DisplayAlerts = True
```

- **カーソルを砂時計にする**
```
Application.Cursor = xlWait 
```
```
Application.Cursor = xlDefault
```

- ページの最初に戻す
```
ActiveWindow.ScrollColumn = 1
ActiveWindow.ScrollRow = 1
Range("A1").Select
```


- 印刷　行トップを必ず入れる
	手順詳細
	
	[ページレイアウト] タブを開く。
	[印刷タイトル] をクリック。
	「シート」タブの [タイトル行] をクリックし、固定したい行（1行目など）を選択。
	「OK」をクリック。
	[ファイル] > [印刷] の「印刷プレビュー」で確認。

- 別のマクロを実行する

Call 〇プログラム名

filename1 = Range("L1").Value

Range("AC1").Select

    gyo1 = ActiveCell.Value

range1 = "AC2:AL" & gyo1

- Vlookupの代わり　index(    match())

- シート　非表示　表示

    Sheets("★伝票").Visible = False

    Sheets("★伝票").Visible = True

- シートの数

=SHEETS()

- シート取得

    Dim sheet As String

       sheet = **_ActiveSheet.Name_**

    Sheets(sheet).Select

- シートの名前がC3にあってそのシートのN1の数値を取る場合　  
=IF($C3="","",INDIRECT($C3&"!N1"))

- セル位置に関して

ROW()   行番号を返す関数

COLUMN()　　列番号を返す関数

- 行や列の最終が何番目かを取得する（空白があっても最終を探す）

・D行の空白ではない一番下のセルの数字

=MAX(INDEX((LEN(D1:D10000)>0)*ROW(D1:D10000),0))

・6行目の空白ではない一番右のセルの数字（範囲はA列～AM列の場合）

=MATCH(9E+307,A6:AM6)+COLUMN(A6)-1

  応用　列番号をアルファベットに変換する方法

=LEFT(ADDRESS(ROW(),COLUMN(),4,1),LEN(ADDRESS(ROW(),COLUMN(),4,1))-LEN(ROW()))

COLUMN()にMATCH(9E+307,A6:AM6)+COLUMN(A6)-1を入れることで、列のアルファベットを取得できる

=LEFT(ADDRESS(ROW(),MATCH(9E+307,A6:AM6)+COLUMN(A6)-1,4,1),LEN(ADDRESS(ROW(),MATCH(9E+307,A6:AM6)+COLUMN(A6)-1,4,1))-LEN(ROW()))

- 名前などの半角をなくす  
=TRIM(SUBSTITUTE(SUBSTITUTE(IFERROR(E2,""),"　","")," ",""))

- パス名取得

    Dim pathname As String

    pathname = ThisWorkbook.Path

- ファイル名　ディレクトリ

              Dim pathname as string

              Dim filename as string

              Dim sheetname as string

              Dim allname as string

=CELL("filename")　　ディレクトリとファイル、シート名含む

・ファイル名=MID(CELL("filename"),FIND("[",CELL("filename"))+1,FIND("]",CELL("filename"))-FIND("[",CELL("filename"))-1)

・シート名

=RIGHT(CELL("filename",A1),LEN(CELL("filename",A1))-FIND("]",CELL("filename",A1)))

●    パス、ファイル名、シート名取得プログラム　元データシートのC1セルが自由に利用できる場合

Sub ファイル名取得

    Dim pathname As String

    Dim filename As String

    Dim sheetname As String

    pathname = ThisWorkbook.Path

    Sheets("元データ").Select

    ActiveSheet.Unprotect

    Range("C1").Select

    ActiveCell.FormulaR1C1 = _

        "=MID(CELL(""filename""),FIND(""["",CELL(""filename""))+1,FIND(""]"",CELL(""filename""))-FIND(""["",CELL(""filename""))-1)"

    filename = ActiveCell.Value

    ActiveCell.FormulaR1C1 = _

        "=RIGHT(CELL(""filename"",RC[-3]),LEN(CELL(""filename"",RC[-3]))-FIND(""]"",CELL(""filename"",RC[-3])))"

    sheetname = ActiveCell.Value

    Range("C1").Select

    Selection.ClearContents

    ActiveSheet.Protect

    Range("A1").Select

End Sub

セキュリティの警告　マクロが無効にされました。　「コンテンツの有効化」ボタンを押す

このファイルを信頼済みドキュメントにしますか？　　はいボタンを押す。

192.168.11.135