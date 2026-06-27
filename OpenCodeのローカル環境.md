

OpenCodeとは？
OpenCodeはターミナル上で動作するAIコーディングエージェントです。
Claude Codeに似た操作感で利用でき、複数のLLMプロバイダーに対応しています。
ローカル環境で完結するように Ollama と組み合わせて利用してみます。

WSL2（Linux）に OpenCode をインストール
まずは WSL2（Linux）環境にOpenCodeをインストールします。

curl -fsSL https://opencode.ai/install | bash

インストール後は opencode コマンドが利用できるようになります。
パスの設定をシェルを再起動せずに即時反映させるため、以下のコマンドを実行しておきます。

source ~/.zshrc  # bashの場合は ~/.bashrc

OpenCodeを起動
インストール後は以下で起動できます。

opencode

WSL2からWindows上のOllamaへ接続する
OpenCode を WSL2 上で動かし、Ollama は Windows 側で動かしています。
そのため、Ollama が外部からアクセスできるよう設定します。
PowerShell（管理者権限）で以下を実行します。

[System.Environment]::SetEnvironmentVariable("OLLAMA_HOST", "0.0.0.0", "User")

設定後に Ollama を再起動します。
Windows Defender Firewall の確認ダイアログが表示された場合はアクセスを許可します。

接続確認
WSL2 側から Windows ホストの IP を確認します。

WINDOWS_IP=$(ip route show | grep -i default | awk '{ print $3}')
echo "Windows IP: ${WINDOWS_IP}"

続いて Ollama API にアクセスしてみます。

curl http://${WINDOWS_IP}:11434/api/tags

モデル一覧が JSON 形式で返ってくれば接続成功です。

Windows の Ollama に qwen2.5-coder:7b をダウンロード
続いて、Windows側の Ollama にモデルをダウンロードします。
今回はコード生成向けに学習された qwen2.5-coder:7b を利用します。

ollama pull qwen2.5-coder:7b

7Bモデルのため比較的軽量で、CPU環境やエントリークラスのGPUでも動作させやすいのが特徴です。OpenCodeの動作確認や日常的なコーディング支援を試すには十分な性能を持っています。

OpenCodeの設定
WSL2側の設定ファイルを作成または編集します。

~/.config/opencode/opencode.json

設定例は以下の通りです。

{
  "$schema": "https://opencode.ai/config.json",

  "provider": {
    "ollama": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Ollama (Windows)",

      "options": {
        "baseURL": "http://<WINDOWS_IP>:11434/v1"
      },

      "models": {
        "qwen2.5-coder:7b": {
          "name": "Qwen2.5 Coder 7B",
          "limit": {
            "context": 32768,
            "output": 4096
          }
        }
      }
    }
  },

  "model": "ollama/qwen2.5-coder:7b",
  "small_model": "ollama/qwen2.5-coder:7b"
}

※ baseURL のIPアドレスは環境によって異なります。前述の接続確認で取得した Windows ホストのIPアドレスを指定してください。

モデルを確認する
OpenCode を起動します。

opencode

起動後に /models コマンドを実行します。
利用可能なモデル一覧が表示されるので、設定したモデルが選択されていることを確認します。


コード生成を試してみる
試しに以下のようなプロンプトを入力してみます。

Pythonで素数を判定するスクリプトを作成して

すると OpenCode がコードを生成してくれます。


ローカル環境だけで完結しているため、API利用料を気にせず試せる点も魅力です。

まとめ
今回は OpenCode と Ollama を組み合わせてローカルAIコーディング環境を構築してみました。

WSL2 と Windows ホスト間のネットワークの壁さえクリアすれば、OpenCode × Ollama の組み合わせで完全ローカルなAIコーディング環境を構築できます。