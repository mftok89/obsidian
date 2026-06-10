obsidianの設定
Local REST API with MCPをコミュニティプラグインで、インストール
（作者 adam coddington）

Obsidianで 設定 → Local REST API を開いて、以下を確認してください：
1. API Key（長い文字列）をコピー
2. Port（デフォルト 27124 のはず）
	   ※　Encrypted （HTTPS）MCP endpoint　の下に、
	   https://127.0.0.1:27124/mcp/
	   のような形で表記されている



---

WSL再起動の手順

1. このClaude Codeを終了する（Ctrl+C または /exit）
2. Windowsの PowerShell を開いて実行：
wsl --shutdown
3. ターミナル（WSL）を開き直す
4. claude を起動して「Obsidian MCPの続きをお願いします」と伝えてください
