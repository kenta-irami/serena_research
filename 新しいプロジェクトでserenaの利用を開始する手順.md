<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# 新しいプロジェクトでserenaの利用を開始する手順を教えてほしい。インストールは済んでる。

新しいプロジェクトでSerenaの利用を開始する手順は以下の通りです（インストール済みの場合）：

1. **プロジェクトのディレクトリに移動**
ターミナルで利用したい新しいプロジェクトフォルダに移動します。
2. **プロジェクトのアクティベート（有効化）**
以下のコマンドを実行してSerenaのMCPサーバーをそのプロジェクトに対して起動し、アクティベートします。

```
uvx --from git+https://github.com/oraios/serena serena-mcp-server --project "$(pwd)"
```

実行後、Ctrl + Cで一旦終了できます。この操作でプロジェクト情報が `~/.serena/serena_config.yml` に登録されます。
3. **設定ファイルの生成**
プロジェクト内にSerena用の設定ファイル `.serena/project.yml` を生成します。コマンドは以下です。

```
uvx --from git+https://github.com/oraios/serena serena project generate-yml
```

生成された設定ファイル内で言語設定（例：`language: csharp`）などを必要に応じて確認・編集してください。
4. **プロジェクトのインデックス生成**
コード解析などの高速化のためにプロジェクトのインデックスを作成します。

```
uvx --from git+https://github.com/oraios/serena serena project index
```

大規模プロジェクトは特に必須です。
5. **Claude Code上などでSerenaを使う場合の設定追加（任意）**
Claude CodeなどのMCPクライアントでSerenaを使う場合、MCP設定にSerenaを追加して起動します。例：

```
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant --project $(pwd)
```

これにより、SerenaをIDEなどから利用しやすくなります。
6. **利用開始**
プロジェクトが有効化され、設定とインデックスが整えばSerenaを使ってコード解析や支援が可能になります。
プロジェクトの切り替え時は改めて「プロジェクト〇〇を有効化してください」といった操作が必要です。

簡単にいうと、

- プロジェクトディレクトリでSerenaのMCPサーバを起動してアクティベート
- 設定ファイルを生成・確認
- インデックスを作成
- 必要に応じてIDE連携の設定も行う

という流れです。環境やクライアントによって細かいコマンドは変動しますが、基本はこれらのステップを踏みます[^1][^2][^4][^5][^6]。

<div style="text-align: center">⁂</div>

[^1]: https://zenn.dev/sator_imaging/articles/24e8e207b2e77e

[^2]: https://note.com/engineers_hub/n/n5d3ad3f617d6

[^3]: https://azukiazusa.dev/blog/serena-coding-agent

[^4]: https://qiita.com/masakinihirota/items/a2c0ef5e6f9a0aa868d1

[^5]: https://apidog.com/jp/blog/serena-mcp-server/

[^6]: https://light11.hatenadiary.com/entry/2025/08/03/184520

[^7]: https://www.microfocus.com/documentation/pvcs-version-manager/8.1.1/JA/vmgs.pdf

[^8]: https://zenn.dev/acntechjp/articles/a3daf17083277f

[^9]: https://note.com/syogaku/n/n721b324b3ca6

[^10]: https://www.microfocus.com/documentation/pvcs-version-manager/8.2.1/JA/VMUG.PDF

