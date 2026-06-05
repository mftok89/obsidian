---
title: Ubuntu でよく使うコマンドまとめ｜npaka
source: https://note.com/npaka/n/n2542f00ec4ec
author:
  - "[[npaka]]"
published: 2025-10-05
created: 2025-10-09
description: "Ubuntu でよく使うコマンドをまとめました。   ・Ubuntu 22.04    1. バージョンの確認  1-1. Ubuntuのバージョン  lsb_release -a  No LSB modules are available.Distributor ID: UbuntuDescription:    Ubuntu 22.04.5 LTSRelease:        22.04Codename:       jammy  1-2. CUDA Toolkit のバージョン  CUDAアプリやライブラリをコンパイルするときに使用するライブラリのCUDAバージョン。"
tags:
  - clippings
  - ノウハウ
  - ubuntu
---
![見出し画像](https://assets.st-note.com/production/uploads/images/220091521/rectangle_large_type_2_0dde149ebf13fae4961bcbdc5e5c0bd6.png?width=1200)







Ubuntu でよく使うコマンドをまとめました。

> **・Ubuntu 22.04**

## 1\. バージョンの確認

### 1-1. Ubuntuのバージョン

```
lsb_release -a
```

```
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.5 LTS
Release:        22.04
Codename:       jammy
```

### 1-2. CUDA Toolkit のバージョン

CUDAアプリやライブラリをコンパイルするときに使用するライブラリのCUDAバージョン。

```
nvcc --version
```

### 1-3. ドライバが対応しているCUDAのバージョン

GPUドライバがサポートする最大のCUDAバージョン。Toolkit/Runtime のバージョン以上でなければならない。

```
nvidia-smi
```

```ruby
Sun Oct  5 11:34:01 2025       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 565.57.01              Driver Version: 565.57.01      CUDA Version: 12.7     |
|-----------------------------------------+------------------------+----------------------+
```

### 1-4. CUDA Runtime のバージョン

コンパイル済みのCUDAアプリを実行するときに使用するライブラリのCUDAバージョン。Toolkit のバージョン以上でなければならない。

**・PyTorchの場合**

```swift
import torch
print(torch.version.cuda)
```

```
12.4
```

## 2\. 容量の確認

### 2-1. GPU（VRAM）の容量

```
nvidia-smi
```

```ruby
Sun Oct  5 12:00:24 2025       
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 565.57.01              Driver Version: 565.57.01      CUDA Version: 12.7     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 3080 ...    Off |   00000000:01:00.0 Off |                  N/A |
| N/A   40C    P0             20W /   80W |      15MiB /  16384MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
```

> **・1 GiB = 1024 MiB**

### 2-2. CPU（メインメモリ）の容量

```cpp
free -h
```

### 2-3. ストレージの容量

```
df -h
```

```cs
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           3.2G  2.3M  3.2G   1% /run
/dev/nvme0n1p2  938G  105G  786G  12% /
tmpfs            16G  518M   16G   4% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
efivarfs        192K  118K   70K  63% /sys/firmware/efi/efivars
/dev/nvme0n1p1  511M  6.1M  505M   2% /boot/efi
tmpfs           3.2G  128K  3.2G   1% /run/user/1000
```

## 3\. アプリの追加 ・ 削除

### 3-1. アプリのインストール

```
sudo apt install <パッケージ名>
```

### 3-2. アプリのアンインストール

```cs
sudo apt remove <パッケージ名>
```

設定ファイルも含めて完全に削除したい場合

```
sudo apt purge <パッケージ名>
```

### 3-3. インストール済みアプリ一覧

```php
apt list --installed
```

### 3-4. パッケージ情報の更新

最新のパッケージ情報を取得します (実際のアップデートはまだ行わない)。

```
sudo apt update
```

**3-5. パッケージのアップグレード**

```
sudo apt upgrade
```

古い依存関係を自動で削除したい場合

```
sudo apt clean
sudo apt autoremove
```

## 4\. Python

### 4-1. Pythonのバージョン

```
python --version
```

```
Python 3.10.12
```

### 4-2. Pythonの仮想環境の作成

仮想環境名がenvの場合

```
python -m venv env
```

### 4-3. Pythonの仮想環境のアクティベート

仮想環境名がenvの場合

```
source env/bin/activate
```

### 4-4. Pythonの仮想環境のデアクティベート

```
deactivate
```

### 4-5. PyTorchのインストール

CUDA 12.4の場合

```javascript
pip install --upgrade pip
pip install torch --index-url https://download.pytorch.org/whl/cu124
```

```python
python
>>> import torch
>>> print(torch.version.cuda)
12.4
```

## 5\. conda

### 5-1. condaのバージョン

```
conda --version
```

### 5-2. condaの仮想環境一覧

```php
conda env list
```

### 5-3. condaの仮想環境の作成

仮想環境名がtorch124の場合

```
conda create -n torch124 python=3.10
```

### 5-4. condaの仮想環境のアクティベート

仮想環境名がtorch124の場合

```
conda activate torch124
```

### 5-5. condaの仮想環境のデアクティベート

```
conda deactivate
```

### 5-6. PyTorchのインストール

CUDA 12.4の場合

```swift
conda install pytorch pytorch-cuda=12.4 -c pytorch -c nvidia
conda install numpy
```

```python
python
>>> import torch
>>> print(torch.version.cuda)
12.4
```

## 6\. フォルダ構成

### 6-1. ルート直下

```php
/
├── bin/        # 基本コマンド
├── etc/        # 設定ファイル
├── home/       
│   ├── ユーザー名/ # ユーザーのホームフォルダ
│   └── guest/
├── lib/        # システムライブラリ
├── usr/
│   ├── bin/    # 一般ユーザー用アプリケーション
│   ├── lib/    # システム管理アプリケーション
│   └── local/  # 手動インストールしたアプリケーション
├── var/
│   ├── log/    # ログ
│   └── cache/  # キャッシュ
├── tmp/        # 一時ファイル
└── boot/       # 起動関連ファイル
```

### 6-2. ホーム直下

```php
/home/ユーザー名/
├── Desktop/        # デスクトップに表示されるファイル
├── Documents/      # 文書ファイル
├── Downloads/      # ダウンロードしたファイル
├── Music/          # 音楽ファイル
├── Pictures/       # 画像ファイル
├── Videos/         # 動画ファイル
├── Templates/      # テンプレート
├── Public/         # 他ユーザーと共有できる公開フォルダ
├── snap/           # snapパッケージ用データ
├── .config/        # アプリの設定ファイル（隠しフォルダ）
├── .local/         # 個人用ライブラリ・実行ファイルなど
├── .cache/         # アプリのキャッシュ
├── .ssh/           # SSHキー・設定
└── .bashrc         # シェルの設定ファイル（bash用）
```

  

[**はじめてのUbuntu** *www.amazon.co.jp*](https://www.amazon.co.jp/dp/4297152770?tag=npaka-22&linkCode=ogi&th=1&psc=1)

[*2,970 円* (2025年10月05日 21:17時点](https://www.amazon.co.jp/dp/4297152770?tag=npaka-22&linkCode=ogi&th=1&psc=1)

[

Amazon.co.jpで購入する

](https://www.amazon.co.jp/dp/4297152770?tag=npaka-22&linkCode=ogi&th=1&psc=1)

  

- [
	#ubuntu
	](https://note.com/hashtag/ubuntu)

[[コマンドプロンプト]]