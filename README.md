# MPC Server プロジェクト

このリポジトリには、ClaudeやClineなどのAIアシスタントが外部サービスと連携するためのModel Context Protocol (MCP) サーバーが含まれています。

## 含まれるMCPサーバー

### 1. Filesystem MCP Server

ファイルシステムとの対話を可能にするMCPサーバーです。AIアシスタントがファイルの読み書きやディレクトリ操作を行うことができます。

#### 主な機能

- ファイルの読み書き
- ディレクトリの作成・一覧表示・削除
- ファイル/ディレクトリの移動
- ファイル検索
- ファイルメタデータの取得

### 2. Notion MCP Server

NotionのAPIを利用して、AIアシスタントがNotionワークスペースと対話できるようにするMCPサーバーです。

#### 主な機能

- Notionページやデータベースの作成・取得・更新・削除
- ブロックの追加・取得・削除
- データベースのクエリ
- コメントの作成・取得
- ユーザー情報の取得
- 検索機能

### 3. MCP Compass

自然言語クエリを使用してMCPサーバーを検索・推奨するためのサービスです。AIアシスタントが特定のタスクに適したMCPサービスを見つけるのを支援します。

## セットアップ方法

### Filesystem MCP Server のセットアップ

1. **セットアップ**:
   ```bash
   npm install @modelcontextprotocol/server-filesystem
   ```

2. **Clineの設定ファイルを編集**:

   **方法A: NPXを使用**
   ```json
   {
     "mcpServers": {
       "filesystem": {
         "command": "npx",
         "args": [
           "-y",
           "@modelcontextprotocol/server-filesystem",
           "/path/to/allowed/directory1",
           "/path/to/allowed/directory2"
         ],
         "disabled": false,
         "alwaysAllow": []
       }
     }
   }
   ```

   **方法B: Dockerを使用**
   ```json
   {
     "mcpServers": {
       "filesystem": {
         "command": "docker",
         "args": [
           "run",
           "-i",
           "--rm",
           "--mount", "type=bind,src=/path/to/directory1,dst=/projects/directory1",
           "--mount", "type=bind,src=/path/to/directory2,dst=/projects/directory2,ro",
           "mcp/filesystem",
           "/projects"
         ],
         "disabled": false,
         "alwaysAllow": []
       }
     }
   }
   ```
   ※ `ro`フラグを追加すると読み取り専用としてマウントされます。

   **注意**: セキュリティ上の理由から、許可されたディレクトリのみがアクセス可能です。

3. **使用方法**:
   Filesystem MCP Serverが設定されると、AIアシスタントは以下の操作を実行できます:

   - **ファイルの読み込み**: `read_file`ツールを使用してファイルの内容を取得
   - **ファイルの書き込み**: `write_file`ツールを使用してファイルを作成または上書き
   - **ファイルの編集**: `edit_file`ツールを使用してファイルの一部を編集
   - **ディレクトリの操作**: ディレクトリの作成、一覧表示、検索など

   AIアシスタントに「ファイルを読み込んでください」「このファイルを作成してください」などの指示を出すだけで、AIがファイルシステムにアクセスできるようになります。

### Notion MCP Server のセットアップ

1. **Notion インテグレーションの作成**:
   - [Notion Your Integrations ページ](https://www.notion.so/profile/integrations)にアクセス
   - 「New Integration」をクリック
   - インテグレーションに名前を付け、適切な権限を選択

2. **シークレットキーの取得**:
   - インテグレーションから「Internal Integration Token」をコピー

3. **ワークスペースへのインテグレーション追加**:
   - アクセスさせたいページやデータベースをNotionで開く
   - 右上のナビゲーションボタンをクリック
   - 「Connect to」ボタンをクリックし、作成したインテグレーションを選択

4. **Notion APIトークンの設定**:

   **Clineの設定ファイルを編集**
   ```json
   {
     "mcpServers": {
       "notion": {
         "command": "node",
         "args": ["/path/to/notion/build/index.js"],
         "env": {
           "NOTION_API_TOKEN": "your-integration-token"
         },
         "disabled": false,
         "alwaysAllow": []
       }
     }
   }
   ```

### MCP Compass のセットアップ

1. **ビルド**:
   ```bash
   cd mcp-compass
   npm install
   npm run build
   ```

2. **Clineの設定ファイルを編集**:
   ```json
   {
     "mcpServers": {
       "mcp-compass": {
         "command": "node",
         "args": ["/path/to/mcp-compass/build/index.js"],
         "disabled": false,
         "alwaysAllow": []
       }
     }
   }
   ```

## トラブルシューティング

権限エラーが発生した場合:

1. インテグレーションに必要な権限があることを確認
2. インテグレーションが関連ページやデータベースに招待されていることを確認
3. トークンと設定が正しく設定されていることを確認

## 詳細情報

- 英語版: https://dev.to/suekou/operating-notion-via-claude-desktop-using-mcp-c0h
- 日本語版: https://qiita.com/suekou/items/44c864583f5e3e6325d9

## ライセンス

このプロジェクトはMITライセンスの下で提供されています。詳細はLICENSEファイルを参照してください。
# my-mpc-server
# my-mcp-server
# my-mcp-server
