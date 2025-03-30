# mcp-tools-stack

このリポジトリには、Claude や Cline などの AI アシスタントが外部サービスと連携するための Model Context Protocol (MCP) サーバーが含まれています。

## 含まれる MCP サーバー

### 1. Filesystem MCP Server

ファイルシステムとの対話を可能にする MCP サーバーです。AI アシスタントがファイルの読み書きやディレクトリ操作を行うことができます。

#### 主な機能

- ファイルの読み書き
- ディレクトリの作成・一覧表示・削除
- ファイル/ディレクトリの移動
- ファイル検索
- ファイルメタデータの取得

### 2. Notion MCP Server

Notion の API を利用して、AI アシスタントが Notion ワークスペースと対話できるようにする MCP サーバーです。

#### 主な機能

- Notion ページやデータベースの作成・取得・更新・削除
- ブロックの追加・取得・削除
- データベースのクエリ
- コメントの作成・取得
- ユーザー情報の取得
- 検索機能

### 3. GitHub MCP Server

GitHub の API を利用して、AI アシスタントが GitHub リポジトリと対話できるようにする MCP サーバーです。

#### 主な機能

- リポジトリ情報の取得
- イシューの作成・取得・更新
- プルリクエストの操作
- コミット履歴の取得
- コードの検索・取得

### 4. MCP Compass

自然言語クエリを使用して MCP サーバーを検索・推奨するためのサービスです。AI アシスタントが特定のタスクに適した MCP サービスを見つけるのを支援します。

## セットアップ

### 前提条件

- Node.js (v18 以上)
- npm (v9 以上)

### インストール

1. リポジトリをクローンまたはダウンロードします：

```bash
git clone https://github.com/yourusername/mcp-tools-stack.git
cd mcp-tools-stack
```

2. 依存関係をインストールします：

```bash
npm install
```

#### 3. Claude Desktop との連携

Claude Desktop と連携するには、`claude_desktop_config.json`ファイルを設定する必要があります。

```
  code ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

##### claude_desktop_config.json の記載例

```json
{
  "mcp_servers": [
    {
      "name": "filesystem",
      "command": "npx @modelcontextprotocol/server-filesystem",
      "auto_start": true
    },
    {
      "name": "notion",
      "command": "npx @suekou/mcp-notion-server",
      "auto_start": true,
      "env": {
        "NOTION_API_KEY": "your_notion_api_key_here"
      }
    },
    {
      "name": "github",
      "command": "npx @modelcontextprotocol/server-github",
      "auto_start": true,
      "env": {
        "GITHUB_TOKEN": "your_github_token_here"
      }
    },
    {
      "name": "compass",
      "command": "npx @liuyoshio/mcp-compass",
      "auto_start": true
    }
  ]
}
```

このファイルを Claude Desktop の設定ディレクトリに配置してください。

#### 4. 環境変数の設定

各 MCP サーバーが外部 API と連携するために必要な環境変数を設定してください：

- **Notion MCP Server**: `NOTION_API_KEY` - Notion の API キー
- **GitHub MCP Server**: `GITHUB_TOKEN` - GitHub のパーソナルアクセストークン
