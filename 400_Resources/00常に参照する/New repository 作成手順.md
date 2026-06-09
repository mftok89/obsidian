- wsl2でターミナルを開く
	- windows検索でubuntuと入力

- wsl2 projectsフォルダにリポジトリ名でフォルダ作成
```
cd projects
mkdir new-repository名
cd  new-repository名
```

- Git初期化
```
git init
```

- cursorで開く
```
cursor .
```

- 何らかのファイルを作成
- 最初のコミットをする（コミット名を考える）

- 新しいリポジトリを作成して、プッシュまでを行う
		ローカルリポジトリ  
		↓  
		GitHub作成  
		↓  
		origin登録  
		↓  
		初回push
```
gh repo create ○○リポジトリ名 --private --source=. --remote=origin --push
```

	private を　publicにすれば、公開モードになる
```
gh repo create ○○リポジトリ名 --public --source=. --remote=origin --push
```

---
# 普段の作業

変更後 

```
git add .
git commit -m "コミットの変更内容説明"
git push
```

取得

```
git pull
```