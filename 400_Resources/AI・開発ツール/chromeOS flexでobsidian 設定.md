#ubuntu #ノウハウ

最初にファイルアプリで、共有したいフォルダを、右クリックで、[Linux と共有] を選択します。
## chromeOS Flex  の  Vaultsの位置
	/mnt/chromeos/GoogleDrive/MyDrive/obsidian
	
	cd /mnt/chromeos/GoogleDrive/MyDrive/obsidian

## ターミナルに下記コマンドを打つ
uname -m  (chromeOS flexは　`x86_64`)

Linuxの実行環境がARM64（aarch64）かx64かを確認するには、
ターミナルでコマンドを実行します。
`uname -m`コマンドを使い、
出力が`aarch64`（または`arm64`）ならARM64、
`x86_64`ならx64アーキテクチャです。﻿

ターミナルで確認する方法

	1. **ターミナルを開きます**.
	2. 以下のコマンドを打ち込んで実行します.﻿
	
	ソースコード
```
uname -m
```

1. 出力結果が「`aarch64`」または「`arm64`」であれば、ARM64アーキテクチャの環境です.﻿
2. 出力結果が「`x86_64`」であれば、x64アーキテクチャの環境です.﻿

コマンドの意味﻿

- `uname -m` は、システムが使用しているCPUアーキテクチャ（ハードウェアの形式）を表示するためのコマンドです.

	