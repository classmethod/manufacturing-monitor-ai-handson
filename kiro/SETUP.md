# セットアップガイド

このハンズオンに必要なツールのインストール手順です。Windows環境を対象としています。

## 必須ツール（全チャプター共通）

| ツール                             | バージョン | 提供元                     | 用途                             |
| ---------------------------------- | ---------- | -------------------------- | -------------------------------- |
| [Python](https://www.python.org/)  | 3.12以上   | Python Software Foundation | アプリケーション実行             |
| [uv](https://docs.astral.sh/uv/)   | 0.13.5     | Astral                     | Pythonパッケージ管理・タスク実行 |
| [Kiro](https://kiro.dev/)          | 最新       | Amazon                     | AI駆動開発IDE                    |
| [SQLite3](https://www.sqlite.org/) | 3.x        | SQLite Consortium          | DBレコード確認                   |

## チャプター別の追加ツール

| ツール                                                        | 対象チャプター | 提供元            | 用途               |
| ------------------------------------------------------------- | -------------- | ----------------- | ------------------ |
| [Node.js](https://nodejs.org/)                                | ch3            | OpenJS Foundation | playwright-cli実行 |
| [playwright-cli](https://github.com/microsoft/playwright-cli) | ch3            | Microsoft         | ブラウザ自動操作   |

### Pythonライブラリ一覧

各チャプターディレクトリで `uv sync` を実行すると自動インストールされます。

| ライブラリ    | バージョン | ch1 | ch2 | ch3 | ch4 | ch5 | 用途                        |
| ------------- | ---------- | --- | --- | --- | --- | --- | --------------------------- |
| streamlit     | >=1.66.0   | ✓   | ✓   | ✓   | ✓   | ✓   | UIフレームワーク            |
| pandas        | >=3.0.0    | ✓   | ✓   | ✓   | ✓   | ✓   | データフレーム処理          |
| plotly        | >=7.0.0    | ✓   | ✓   | ✓   | ✓   | ✓   | インタラクティブチャート    |
| openpyxl      | >=3.1.0    | ✓   | ✓   | ✓   | ✓   | ✓   | Excel読み込み               |
| pytest        | >=9.1.0    | ✓   | ✓   | ✓   | ✓   | ✓   | テスト実行                  |
| ruff          | >=0.17.0   |     |     |     | ✓   | ✓   | リンター・フォーマッター    |
| torch         | >=2.14.0   |     |     |     |     | ✓   | PyTorch（推論バックエンド） |
| transformers  | >=5.20.0   |     |     |     |     | ✓   | Hugging Face Transformers   |
| sentencepiece | <0.2.2     |     |     |     |     | ✓   | トークナイザー              |

- sentencepiece は 0.2.2 未満に制限しています。0.2.2 で `SentencePieceProcessor.Init` が削除され、ch5 で使う PLaMo-Embedding のトークナイザーが動作しなくなるためです。
- torch 2.14 は Python 3.15 向けのパッケージを提供していません。ch5 は Python 3.12〜3.14 で実行してください。

## インストール手順

### 1. Python

wingetでインストールします。

```powershell
winget install -e --id Python.Python.3.12
```

インストール後、バージョンを確認します。

```powershell
python --version
# Python 3.12.x と表示されればOK
```

> 公式サイトからインストーラーをダウンロードする場合は、インストール時に「Add Python to PATH」にチェックを入れてください。

### 2. uv

PowerShellで以下を実行します。

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.13.5/install.ps1 | iex"
```

インストール後、ターミナルを再起動してバージョンを確認します。

```powershell
uv --version
# uv 0.13.5 と表示されればOK
```

### 3. Kiro

管理者にライセンスを付与してもらった上で、[公式サイト](https://kiro.dev/)からインストーラーをダウンロードし、画面の指示に従ってインストールしてください。

#### 3.1. ライセンスの確認

Kiro IDE を起動し、画面右下のステータスバーでライセンス情報を確認します。

![Kiro IDE 右下のライセンス表示](../assets/setup/01.png)

拡大すると以下のように表示されます。**Free になっていないこと**（Pro、Pro+、Power のいずれかであること）をご確認ください。

![ライセンス表示の拡大](../assets/setup/02.png)

> ライセンスが正しく付与されているかの確認です。

#### 3.2. チャットが開始できることの確認

右上の吹き出しアイコンからチャットパネルを開きます。

![チャットパネルを開く](../assets/setup/03.png)

**Vibe** モードを選択します。

![Vibeモードを選択](../assets/setup/04.png)

チャット欄に「こんにちは」などメッセージを入力し、応答が返却されることをご確認ください。

![チャットの応答確認](../assets/setup/05.png)

> 生成AIへリクエストが正常に送信できるかの確認です。

### 4. SQLite3

wingetでインストールします。

```powershell
winget install -e --id SQLite.SQLite
```

インストール後、ターミナルを再起動してバージョンを確認します。

```powershell
sqlite3 --version
# 3.x.x と表示されればOK
```

動作確認として、インメモリDBで簡単な操作を試します。

```powershell
sqlite3 ":memory:" "CREATE TABLE test(id INTEGER, name TEXT); INSERT INTO test VALUES(1, 'hello'); SELECT * FROM test;"
# 1|hello と表示されればOK
```

### 5. Node.js（ch3で必要）

wingetでインストールします。

```powershell
winget install -e --id OpenJS.NodeJS.LTS
```

インストール後、バージョンを確認します。

```powershell
node --version
# v22.x.x 以上であればOK
```

### 6. playwright-cli（ch3で必要）

Node.jsインストール後に以下を実行します。

```powershell
npm install -g @playwright/cli@0.1.22
```

ブラウザバイナリをインストールします。

```powershell
npx playwright install
```

動画録画機能を使う場合は、ffmpegも追加でインストールします。

```powershell
npx playwright install ffmpeg
```

インストール後、動作確認します。

```powershell
npx playwright-cli --version
```

各チャプターの起動方法は [README.md](./README.md) のチャプター構成を参照してください。
