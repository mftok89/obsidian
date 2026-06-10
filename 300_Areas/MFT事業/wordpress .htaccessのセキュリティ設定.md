---
title: "wordpress .htaccessのセキュリティ設定"
created: 2026-05-09
tags: []
---


#mft事業 　

blog,jbblのコントロールパネルにアクセスする場合は、.htaccessの前半部分（下記）を一時的に削除してログインする
# BEGIN WordPress の前まで


コントロールパネルのアクセスが必要なくなったら、
再び、下記の内容を　# BEGIN WordPress の前に追記してアップロードする



```

# ===== セキュリティ強化設定 =====

  

# xmlrpc.php を完全ブロック

<Files xmlrpc.php>

    Order Deny,Allow

    Deny from all

</Files>

  

# wp-login.php を自分のIPのみ許可

# ※ YOUR.IP.ADDRESS.HERE を実際のIPアドレスに変更すること

# ※ IPが変動する場合は Allow from all にして下記コメントアウト

<Files wp-login.php>

    Order Deny,Allow

    Deny from all

    Allow from YOUR.IP.ADDRESS.HERE

</Files>

  

# ディレクトリリスティング禁止

Options -Indexes

  

# 設定ファイル類へのアクセス禁止

<FilesMatch "\.(htaccess|htpasswd|ini|log|sh|sql|bak|config)$">

    Order Deny,Allow

    Deny from all

</FilesMatch>

  

# wp-config.php へのアクセス禁止（念のため）

<Files wp-config.php>

    Order Deny,Allow

    Deny from all

</Files>

```