# mastra-deploy

## 概要

このプロジェクトは、Mastra フレームワークで構築された AI 搭載の天気アシスタントです。正確な天気情報を提供し、天気予報に基づいたアクティビティの推奨を行います。

## インストール

1.  **リポジトリのクローン:**

    ```bash
    git clone <repository-url>
    cd mastra-deploy
    ```

2.  **依存関係のインストール:**

    ```bash
    npm install
    ```

**注意:** このプロジェクトは Google の Gemini 1.5 Pro モデルを使用しています。AI 機能が正しく動作するために、必要な API キーが環境に設定されていることを確認してください。

## 使用方法

このアプリケーションは、さまざまなモードで実行できます。

### 開発モード

開発モードでアプリケーションを実行するには:

```bash
npm run dev
```

### ビルド

本番用にアプリケーションをビルドするには:

```bash
npm run build
```

### 起動

ビルドされたアプリケーションを起動するには:

```bash
npm start
```

### 機能

このアプリケーションは主に2つの主要な機能を提供します。

*   **天気情報 (`Weather Agent` 経由):**
    *   指定された場所の現在の天気詳細を提供します。
    *   場所が提供されていない場合は、自動的に場所を尋ねます。
    *   英語以外の場所名を翻訳します。
    *   湿度、風の状態、降水量などの関連する詳細を含みます。
    *   応答は簡潔でありながら有益です。

*   **天気に基づいたアクティビティの推奨 (`Weather Workflow` 経由):**
    *   天気予報データを分析し、実用的なアクティビティを提案します。
    *   推奨事項は、天気概要、午前/午後の屋外アクティビティ、屋内代替案など、日ごとに構成されます。
    *   特定の場所、トレイル、または場所、および気温に基づいたアクティビティの強度を考慮します。

*   **一般情報とウェブ検索 (`MCP Agent` 経由):**
    *   現在の情報のために Brave Search を使用して一般的なウェブ検索を実行します。
    *   Mastra AI フレームワーク、その機能、ドキュメント、および実装の詳細に関する情報を提供します。
    *   ユーザーのクエリに基づいて、最も適切なツール (Brave Search または Mastra ドキュメント) をインテリジェントに選択します。

## 仕組み

このプロジェクトは、Mastra フレームワークを活用して AI エージェントとワークフローを調整します。

*   **`src/mastra/index.ts`**: 定義されたワークフローとエージェントで Mastra アプリケーションを初期化するメインのエントリポイントです。
*   **`src/mastra/agents/weather-agent.ts`**: Google の Gemini 1.5 Pro を搭載した AI エージェントです。`weatherTool` を使用してリアルタイムの天気データを取得し、天気に関するユーザーのクエリに役立つ応答を提供します。
*   **`src/mastra/workflows/weather-workflow.ts`**: 次のステップを実行する構造化されたワークフローです。
    1.  **`fetchWeather`**: Open-Meteo API を使用して、指定された都市の天気予報データを取得します。
    2.  **`planActivities`**: 別の AI エージェント (Gemini 1.5 Pro も) を利用して、取得した天気予報を分析し、明確さと実用性のためにフォーマットされた詳細なアクティビティ推奨を生成します。
*   **`src/mastra/tools/weather-tool.ts`**: `Weather Agent` が外部の天気 API と対話するために使用するカスタムツールです。
*   **`src/mastra/agents/mcp-agent.ts`**: Google の Gemini 2.0 Flash を搭載した AI エージェントです。Model Context Protocol (MCP) を利用して、一般的なウェブクエリ用の Brave Search や、フレームワーク固有の質問用の専用 Mastra ドキュメントツールなどの外部ツールにアクセスします。

## 使用技術

このプロジェクトは以下を使用して構築されています。

*   Node.js (>=20.9.0)
*   TypeScript
*   Mastra Framework
*   AI SDK for Google
*   その他の Mastra 関連ライブラリ (`@mastra/libsql`, `@mastra/loggers`, `@mastra/mcp`, `@mastra/memory`)
*   Zod

## プロジェクト構造

```
C:/Users/user/mastra-deploy/
├───.gitattributes
├───.gitignore
├───package-lock.json
├───package.json
├───.git/...
├───.mastra/...
├───node_modules/...
└───src/
    └───mastra/
        ├───index.ts
        ├───agents/
        │   ├───mcp-agent.ts
        │   └───weather-agent.ts
        ├───tools/
        │   └───weather-tool.ts
        └───workflows/
            └───weather-workflow.ts
```

