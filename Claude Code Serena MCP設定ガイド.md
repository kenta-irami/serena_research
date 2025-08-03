# Claude Code Serena MCP設定ガイド

## 問題の概要

Claude CodeでSerena MCPサーバーを設定する際に以下の問題が発生：

- `uvx --from` オプションが認識されない
- 設定ファイルの構造が壊れる
- 複数のスコープに重複した設定が作成される

## 解決手順

### 1. 既存設定の削除

複数のスコープに設定がある場合、すべて削除します：

```bash
# ローカル設定（プロジェクト固有）を削除
claude mcp remove serena -s local

# ユーザー設定（グローバル）を削除
claude mcp remove serena -s user
```

### 2. 正しい設定で再追加

PowerShellでは `cmd /c` を使用してコマンドを実行します：

```bash
# プロジェクト固有で追加する場合
claude mcp add serena -s local -- cmd /c "uvx --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant --project C:\Shigoto\news-jockey-site"

# または全プロジェクトで使用する場合
claude mcp add serena -- cmd /c "uvx --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant --project C:\Shigoto\news-jockey-site"
```

### 3. 設定の確認

```bash
claude mcp list
```

正常に接続されていることを確認します。

## 手動設定（代替方法）

### 設定ファイルの直接編集

`C:\Users\[ユーザー名]\.claude.json` を編集：

#### プロジェクト固有設定
```json
{
  "projects": {
    "C:/Shigoto/news-jockey-site": {
      "mcpServers": {
        "serena": {
          "type": "stdio",
          "command": "cmd",
          "args": [
            "/c",
            "uvx",
            "--from",
            "git+https://github.com/oraios/serena",
            "serena-mcp-server",
            "--context",
            "ide-assistant",
            "--project",
            "C:\\Shigoto\\news-jockey-site"
          ],
          "env": {}
        }
      }
    }
  }
}
```

#### グローバル設定
```json
{
  "mcpServers": {
    "serena": {
      "type": "stdio",
      "command": "cmd",
      "args": [
        "/c",
        "uvx",
        "--from",
        "git+https://github.com/oraios/serena",
        "serena-mcp-server",
        "--context",
        "ide-assistant",
        "--project",
        "C:\\Shigoto\\news-jockey-site"
      ],
      "env": {}
    }
  }
}
```

### 設定後の作業

1. Claude Codeを完全に再起動
2. 設定が正しく読み込まれていることを確認

## トラブルシューティング

### よくある問題

1. **`--from` オプションエラー**
   - PowerShellでは `cmd /c` を使用する
   - コマンド全体をダブルクォートで囲む

2. **設定ファイルの構造エラー**
   - JSONの構造を確認
   - 不正な入れ子構造を修正

3. **複数スコープの競合**
   - すべてのスコープから設定を削除
   - 一つのスコープに統一して再設定

### 動作確認

```bash
# Serenaが正常に動作するかテスト
uvx --from git+https://github.com/oraios/serena serena-mcp-server --help

# MCP接続状況確認
claude mcp list

# 詳細なデバッグ情報
claude mcp list --verbose
```

## 補足情報

- **プロジェクト固有設定**: `-s local` オプションを使用
- **グローバル設定**: デフォルト（オプションなし）
- **設定ファイル場所**: `C:\Users\[ユーザー名]\.claude.json`
- **再起動の重要性**: 設定変更後は必ずClaude Codeを再起動

## 参考コマンド

```bash
# MCP関連のコマンド一覧
claude mcp --help

# 設定一覧表示
claude mcp list

# 特定サーバーの詳細
claude mcp status serena

# 再接続
claude mcp reconnect serena
```