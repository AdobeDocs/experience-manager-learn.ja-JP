---
title: AEM Assets の Asset compute 拡張機能のトラブルシューティング
description: 以下に、AEM Assets 用のカスタム Asset compute ワーカーを開発およびデプロイする際に発生する可能性のある、一般的な問題とエラーのインデックスと、解決方法を示します。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5802
thumbnail: KT-5802.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: d851d315-ed0e-46b8-bcd8-417e1e58c0c4
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1218'
ht-degree: 100%

---

# Asset Compute 拡張機能のトラブルシューティング

以下に、AEM Assets 用のカスタム Asset compute ワーカーを開発およびデプロイする際に発生する可能性のある、一般的な問題とエラーのインデックスと、解決方法を示します。

## 開発{#develop}

### レンディションが部分的に描画された／破損した状態で返される{#rendition-returned-partially-drawn-or-corrupt}

+ __エラー__：レンディションのレンダリングが不完全（画像の場合）または破損していて開くことができない。

  ![レンディションが部分的に描画された状態で返されます](./assets/troubleshooting/develop__await.png)

+ __原因__：レンディションがに完全に `rendition.path` に書き込まれることができる前に、ワーカーの `renditionCallback` 関数が関数が終了しています。
+ __解決策__：カスタムワーカーコードを確認し、`await` を使用してすべての非同期の呼び出しが確実に同期されるようにします。

## 開発ツール{#development-tool}

### Asset Compute プロジェクトに Console.json ファイルが見つからない{#missing-console-json}

+ __エラー：__ エラー：非同期の setupAssetCompute（`.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY`）の検証時に必要なファイル（`.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY`）がありません
+ __原因：__&#x200B;この `console.json` ファイルが Asset omputeプロジェクトのルートにありません
+ __解決策：__&#x200B;新しい `console.json` を Adobe I/O プロジェクトからダウンロードします
   1. console.adobe.io で、Asset compute プロジェクトが使用するように設定されている Adobe I/O プロジェクトを開きます
   1. 右上隅の「__ダウンロード__」ボタンをタップします
   1. `console.json` というファイル名を使用して、ダウンロードしたファイルを Asset Compute プロジェクトのルートに保存します

### manifest.yml の YAML インデントが正しくない{#incorrect-yaml-indentation}

+ __エラー：__ YAMLException：行 X、列 Y でマッピングエントリの不正なインデント：（`aio app run` コマンドからの標準経由）
+ __原因：__ Yaml ファイルはホワイトスペースで区別されます。インデントが正しくない可能性があります。
+ __解決策：__`manifest.yml` をレビューし、すべてのインデントが正しいことを確認します。

### memorySize の上限の設定値が低すぎる{#memorysize-limit-is-set-too-low}

+ __エラー：__&#x200B;ローカル開発サーバー OpenWhiskError：PUT https://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=true で HTTP 400（無効なリクエスト）が返される -->「リクエストのコンテンツの形式が正しくありません：要求が失敗しました：メモリ 64 MB が許容されるしきい値の 134217728 B を下回っています」
+ __原因：__`manifest.yml` のワーカーの `memorySize` の制限が、エラーメッセージでバイト単位で報告されているように、許可される最小しきい値を下回って設定されています。
+ __解決策：__`manifest.yml` の `memorySize` の制限をレビューし、いずれも許容される最小しきい値を超えていることを確認します。

### private.key が見つからず、開発ツールを開始できない場合{#missing-private-key}

+ __エラー：__&#x200B;ローカル開発サーバーエラー：validatePrivateKeyFile に必要なファイルがありません…。（`aio app run` コマンドからの標準経由）
+ __原因：__`.env` ファイルの `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 値で、`private.key` または `private.key` を指しておらず、現在のユーザーによって読み取り可能ではありません。
+ __解決策：__`.env` ファイルの `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 値をレビューし、ファイルシステムの `private.key` の完全な絶対パスを含んでいることを確認します。

### ソースファイルのドロップダウンが正しくない{#source-files-dropdown-incorrect}

Asset Compute 開発ツールが古いデータを取り込む状態に入る場合があり、そのことが最も気づかれやすいのは、「__ソースファイル__」ドロップダウンに間違った項目が表示されていることによります。

+ __エラー：__「ソースファイル」ドロップダウンに正しくない項目が表示される。
+ __原因：__&#x200B;キャッシュされたブラウザーの状態が古いことが原因です
+ __解決策：__&#x200B;ブラウザーで、ブラウザータブの「アプリケーションの状態」、ブラウザーキャッシュ、ローカルストレージ、サービスワーカーを完全にクリアします。

### devToolToken クエリパラメーターがないか無効{#missing-or-invalid-devtooltoken-query-parameter}

+ __エラー：__ Asset Compute 開発ツールでの「未認証」通知
+ __原因：__`devToolToken` が見つからないか無効です
+ __解決策：__「Asset compute 開発ツール」ブラウザーウィンドウを閉じ、`aio app run` コマンドによって開始された開発ツールの実行中のプロセスをすべて終了して、開発ツールを（`aio app run` を使用して）再起動します。

### ソースファイルを削除できない{#unable-to-remove-source-files}

+ __エラー：__&#x200B;開発ツール UI から追加されたソースファイルを削除する方法がない
+ __原因：__&#x200B;この機能は実装されていません
+ __解決策：__`.env` で定義された資格情報を使用して、クラウドストレージプロバイダーにログインします。開発ツールで使用されている（および `.env` で指定されている）コンテナを特定し、__ソース__&#x200B;フォルダーに移動して、ソース画像をすべて削除します。削除されたソースファイルが引き続きドロップダウンに表示される場合は、開発ツールの「アプリケーションの状態」でローカルにキャッシュされている可能性があるので、「[ソースファイルのドロップダウンが正しくありません](#source-files-dropdown-incorrect)」にまとめられている手順を実行することが必要な場合があります。

  ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## テスト{#test}

### テストの実行中にレンディションが生成されませんでした{#test-no-rendition-generated}

+ __エラー：__&#x200B;失敗：レンディションが生成されませんでした。
+ __原因：__ JavaScript 構文エラーなどの予期しないエラーにより、ワーカーはレンディションの生成に失敗しました。
+ __解決策：__`/build/test-results/test-worker/test.log` でテストの実行の `test.log` を確認します。このファイル内で、失敗したテストケースに対応するセクションを探し、エラーを確認します。

  ![トラブルシューティング - レンディションが生成されていません](./assets/troubleshooting/test__no-rendition-generated.png)

### テストで、間違ったレンディションが生成され、テストが失敗する{#tests-generates-incorrect-rendition}

+ __エラー：__ 失敗：レンディション「rendition.xxx」が想定通りでない。
+ __原因：__ ワーカーで出力されるレンディションが、テストケースで提供された `rendition.<extension>` とは異なります。
   + 期待される `rendition.<extension>` ファイルがテストケースでローカルに生成されたレンディションと完全に同じ方法で作成されていない場合、ビットに何らかの違いがあるので、テストが失敗する可能性があります。 例えば、Asset Compute ワーカーが API を使用してコントラストを変更し、期待される結果が Adobe Photoshop CC でコントラストを調整して作成された場合、ファイルは同じように見えても、ビットの小さなバリエーションは異なる場合があります。
+ __解像度：__ `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` に移動して、テストからのレンディション出力を確認し、テストケースで期待されるレンディションファイルと比較します。正確に期待されるアセットを作成するには、次のいずれかを実行します。
   + 開発ツールを使用して、レンディションを生成し、正しいことを検証し、期待されるレンディションファイルとして使用します
   + または、`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>` でテスト生成ファイルを検証します。正しいことを検証し、それを期待されるレンディションファイルとして使用します。

## デバッグ

### デバッガーがアタッチしない{#debugger-does-not-attach}

+ __エラー__：Experience Platform Launch の処理中にエラーが発生しました：エラー：次の場所でデバッグターゲットに接続できませんでした…
+ __原因__：Docker デスクトップがローカルシステムで動作していません。VS Code Debug コンソール（表示／デバッグコンソール）を確認し、このエラーが報告されていることを確認します。
+ __解像度__：[Docker デスクトップを開始して、必要な Docker イメージがインストールされていることを確認します](./set-up/development-environment.md#docker)。

### ブレークポイントが一時停止しない{#breakpoints-no-pausing}

+ __エラー__：デバッグ可能な開発ツールから Asset Compute ワーカーを実行する場合、VS Code がブレークポイントで一時停止しない。

#### VS Code デバッガーがアタッチされていない{#vs-code-debugger-not-attached}

+ __原因__：VS Code デバッガが停止／切断されました。
+ __解決策__： VS Code デバッガーを再起動し、VS Codeデバッグ出力コンソール（表示／デバッグコンソール）を見てアタッチすることを確認します。

#### ワーカーの実行が開始した後にVS コードデバッガーがアタッチされる{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因__：VS Code デバッガーは、開発ツール内で「__実行__」をタップする前に接続されませんでした。
+ __解決策__：VS Code のデバッグコンソール（表示／デバッグコンソール）を確認し、開発ツールから Asset Compute ワーカーを再実行して、デバッガーが接続されていることを確認します。

### デバッグ中にワーカーがタイムアウトする{#worker-times-out-while-debugging}

+ __エラー__：デバッグコンソールレポートの「-XXX ミリ秒でアクションがタイムアウトします」、または [Asset Compute 開発ツールの ](./develop/development-tool.md) レンディションプレビューが無期限にスピンする、または、
+ __原因__： [manifest.yml](./develop/manifest.md) で定義されたワーカーのタイムアウトが デバッグ中に超過しています。
+ __解決策__：ワーカーのタイムアウトを一時的に [manifest.yml](./develop/manifest.md) で増加する、またはデバッグアクティビティを高速化します。

### デバッガープロセスを終了できない{#cannot-terminate-debugger-process}

+ __エラー__:：コマンドラインの `Ctrl-C` でデバッガプロセス（`npx adobe-asset-compute devtool`）が終了しません。
+ __原因__：`@adobe/aio-cli-plugin-asset-compute` 1.3.x のバグによって、`Ctrl-C` が終了コマンドとして認識されていません。
+ __解決策__：`@adobe/aio-cli-plugin-asset-compute` をバージョン 1.4.1 以降に更新します

  ```
  $ aio update
  ```

  ![トラブルシューティング - aio の更新](./assets/troubleshooting/debug__terminate.png)

## デプロイ{#deploy}

### AEM のアセットにカスタムレンディションがない{#custom-rendition-missing-from-asset}

+ __エラー__：新しいアセットと再処理されたアセットは正常に処理されましたが、カスタムレンディションが見つかりません

#### 上位フォルダーに処理プロファイルが適用されていない

+ __原因__：カスタムワーカーを使用する処理プロファイルを含むフォルダーの下にアセットが存在しません
+ __解決策__：処理プロファイルをアセットの上位フォルダーに適用します

#### 処理プロファイルが下位の処理プロファイルに置き換えられる

+ __原因__：アセットは、カスタムワーカーの処理プロファイルが適用されたフォルダーの下に存在しますが、そのフォルダーとアセットの間に、顧客のワーカーを使用しない別の処理プロファイルが適用されています。
+ __解決策__：2 つの処理プロファイルを組み合わせるか、紐付けを行い、中間の処理プロファイルを削除します

### AEM でのアセット処理が失敗する{#asset-processing-fails}

+ __エラー__：アセットに「アセット処理失敗」バッジが表示される
+ __原因__：カスタムワーカーの実行中にエラーが発生しました
+ __解決策__：`aio app logs` を使用した [Adobe I/O Runtime アクティベーションのデバッグ](./test-debug/debug.md#aio-app-logs) の指示に従います。
