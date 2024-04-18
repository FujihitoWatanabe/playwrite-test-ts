# Playwright

Playwright を使用したテストスイートの設定手順について説明します。

## 目次

- [事前準備](#事前準備)
- [インストール](#インストール)
  - [初めてインストールする際に表示される対話](#初めてインストールする際に表示される対話)
- [テストの実行](#テストの実行)
- [参考リンク](#参考リンク)

## 事前準備

Node.js 、 npm 、Visual Studio Code を事前にインストールしておいてください。
[Node のバージョン](https://learn.microsoft.com/ja-jp/microsoft-edge/playwright/)はバージョン 12 以上の Node.js が必要です。

## インストール

Playwright をプロジェクトにインストールするには、2 つの方法があります。

1. `npm init playwright@latest`

このコマンドを実行すると、Playwright の初期設定が自動的に行われます。
プロジェクトに必要なファイル/フォルダ構造が作成され、Playwright のインストールも同時に行われます。
初めて Playwright を使う場合は、このコマンドを使うと便利です。

2. `npm install playwright --save-dev`

このコマンドは、Playwright ライブラリをプロジェクトの開発依存関係として追加します。
`--save-dev`オプションにより、Playwright がテスト実行時にのみ必要なライブラリであることが明確になります。

プロジェクトに既に Playwright を使っている場合や、手動で Playwright の設定を行いたい場合は、
このコマンドを使って Playwright をインストールすることをおすすめします。

どちらのコマンドを使うかは、プロジェクトの状況に応じて選択してください。

### 初めてインストールする際に表示される対話

初めて playwright をインストールする際に対話で質問されるので選択していきます。

コンフィグファイルなど使用するファイル形式を TypeScript か JavaScript かの選択をします。このリポジトリでは TypeScript を選択しています。

```
? Do you want to use TypeScript or JavaScript? ...
> TypeScript
  JavaScript
```

テストファイルを設置するディレクトリ名を決めます。変更しないのであればエンターキーを押下して「tests」とします。

```
Where to put your end-to-end tests? » tests
```

GitHub Actions の workflow の追加を聞かれます。デフォルトでは N の false、y で true となります。GitHub Actions の workflow を利用する予定があるのであれば y を選択してエンターキーを押下。利用する予定がないのであればそのままエンターキーを押下します。

```
? Add a GitHub Actions workflow? (y/N) » false
```

テストで利用するブラウザをインストールするかを聞かれています。Y がデフォルトの true となります。n で false となります。デフォルトの Y のままエンターキーを押下してブラウザを利用できるようにします。

```
? Install Playwright browsers (can be done manually via 'npx playwright install')? (Y/n) » true
```

## テストの実行

プロジェクトのルートディレクトリに tests ディレクトリを作成し、そこにテストファイルを配置します。
テストファイルには以下のような構造を設けることをおすすめします。

tests/

- login.spec.js
- product.spec.js
- checkout.spec.js

コマンドを実行できるように package.json の scripts を下記のようにします。

```
  "scripts": {
    "playwright": "playwright test",
    "playwright:ui": "playwright test--ui",
    "playwright:report": "playwright show-report"
  },
```

テストを実行するには以下のコマンドを使用します。

`npx playwright test`

上記のコマンドはブラウザをヘッドレスモードで起動します。
ヘッドレスモードとは、ブラウザの GUI (グラフィカルユーザーインターフェース) を表示せずに、バックグラウンドで実行されるモードです。
ヘッドレスモードでは、ブラウザの起動が速く、リソース消費も少ないため、CI/CD 環境などでの活用に適しています。
ただし、ブラウザの動作を目視で確認できないため、デバッグが困難になる可能性があります。

以下のコマンドはブラウザをヘッドフルモードで起動します。
ヘッドフルモードとは、ブラウザの GUI を表示させるモードです。
ブラウザの動作を目視で確認できるため、テストの開発やデバッグ時に便利です。
ただし、ヘッドレスモードに比べて起動時間が遅く、リソース消費も多くなる可能性があります。

`npx playwright test --headed`

テスト結果の出力を html のレポートとして確認したい場合は以下のコマンドを使用。

`npx playwright show-report`

テストの実行結果が HTML 形式のレポートとして表示されます。レポートには以下のような情報が含まれています。

- テストの概要 (合格/失敗の件数など)
- 各テストケースの詳細 (テスト名、ステータス、実行時間など)
- スクリーンショットなどの追加情報 (テスト実行時に生成されたスクリーンショットなど)

このレポートを確認することで、テストの実行結果を視覚的に把握することができ、テスト失敗時のデバッグにも役立ちます。

UI モードテストを実行したい場合は以下のコマンドを利用

`npx playwright test --ui`

上記の UI モードでは、Playwright の専用ブラウザウィンドウが立ち上がり、テストの実行過程を視覚的に確認できます。
テストケースの実行状況や、ブラウザ上での挙動を直接確認したい場合に便利です。
また、テスト中にデバッグのために一時停止したり、ステップ実行することも可能です。

UI モードは主に開発者が自身のテストを確認する際に利用されますが、
複雑な UI 操作を伴うテストケースの作成や、デバッグ時の活用など、様々な場面で役立ちます。

特定のテストファイルを実行したい場合は、以下のようにファイル名を指定します。

`npx playwright test login.spec.js`

## 参考リンク

- [ドキュメント](https://playwright.dev/docs/intro)

- [テストの書き方](https://playwright.dev/docs/writing-tests)

- [コマンドラインツール](https://playwright.dev/docs/browsers)
