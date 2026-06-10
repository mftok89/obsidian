---
title: "WSL新規設定必須　Zone.Identifier問題"
created: 2026-06-09
tags: []
---

## 1. cursorでZone.Identifierを表示させないようにする

### cursorのsettings.jsonファイルを変更する

`Ctrl + Shift + P`

↓

```
Preferences: Open User Settings (JSON)
```

↓

開いた設定ファイルに追加==（※普通に行うと難しいので、ここは概念だけで、次を参照してください。）==

```
{  "files.exclude": {    "**/*:Zone.Identifier": true  }}
```

- 元のjsonファイル
```
{
    "window.autoDetectColorScheme": true,
    "workbench.preferredLightColorTheme": "Cursor Dark",
    "git.autofetch": true,
    "files.eol": "\n",
    "terminal.integrated.mouseWheelScrollSensitivity": 3,
    "terminal.integrated.gpuAcceleration": "off",
    "terminal.integrated.profiles.windows": {
        "Git Bash": {
            "source": "Git Bash"
        },
        "PowerShell": {
            "source": "PowerShell"
        }
    },
    "terminal.integrated.defaultProfile.windows": "Git Bash"
}
```
- 変更後のjsonファイル
```
{
    "window.autoDetectColorScheme": true,
    "workbench.preferredLightColorTheme": "Cursor Dark",
    "git.autofetch": true,
    "files.eol": "\n",

    "files.exclude": {
        "**/*:Zone.Identifier": true
    },

    "terminal.integrated.mouseWheelScrollSensitivity": 3,
    "terminal.integrated.gpuAcceleration": "off",
    "terminal.integrated.profiles.windows": {
        "Git Bash": {
            "source": "Git Bash"
        },
        "PowerShell": {
            "source": "PowerShell"
        }
    },
    "terminal.integrated.defaultProfile.windows": "Git Bash"
}
```

Zone.Identifierファイルが表示されなくなれば成功です。

---
## 2.グローバルgitignoreを利用して、gitに反映されないようにする

次のステップとしては、Gitも使っているのであれば、WSLで一度だけ以下を設定しておくとさらに快適になります。

WSLで

```
touch ~/.gitignore_global
```

```
echo '*:Zone.Identifier' >> ~/.gitignore_global
```

```
git config --global core.excludesfile ~/.gitignore_global
```

これで今後作るすべての Git リポジトリで無視されます。

==\\wsl.localhost\Ubuntu\home\osamuフォルダに、==
==.gitignore_globalファイルが作成される==