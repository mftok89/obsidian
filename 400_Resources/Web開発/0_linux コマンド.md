## 最新アップデート sudo apt update sudo apt upgrade

	sudo apt update && sudo apt upgrade

## ディレクトリの移動 cd
	cd 〇〇フォルダ
	Gドライブで開くには、 
	G:　　と入力する

## 実行権限を付与する
	chmod a+x 〇〇 
	例 chmod a+x cryptomator-1.15.1-x86_64.AppImage
		chmod a+x cryptomator.AppImage
		chmod a+x obsidian.AppImage
		chmod a+x crypto.AppImage

## AppImage（cryotomator）を実行する
	./crypto.AppImage

## ディレクトリのファイル確認
- **`ls`**: カレントディレクトリのファイルやディレクトリを一覧表示
- **`ls -l`**: ファイル名、パーミッション、所有者、更新日時など表示
- **`ls -a`**: 隠しファイル（ファイル名が`.`で始まるもの）も表示
- **`ls [ディレクトリ名]`**: 指定したディレクトリの中身を表示