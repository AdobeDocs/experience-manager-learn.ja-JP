---
title: AEM AssetsのAsset compute拡張機能のトラブルシューティング
description: 次に、AEM Assets用のカスタムAsset computeワーカーを開発およびデプロイする際に発生する可能性のある、一般的な問題とエラーのインデックスと、解決策を示します。
feature: asset computeマイクロサービス
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 0%

---


# asset computeの拡張機能のトラブルシューティング

次に、AEM Assets用のカスタムAsset computeワーカーを開発およびデプロイする際に発生する可能性のある、一般的な問題とエラーのインデックスと、解決策を示します。

## 開発{#develop}

### レンディションが部分的に描画/破損した{#rendition-returned-partially-drawn-or-corrupt}

+ __エラー__:レンディションが（画像の場合）完全にレンダリングされない、または壊れていて開けない。

   ![レンディションが部分的に描画されて返される](./assets/troubleshooting/develop__await.png)

+ __原因__:レンディションの `renditionCallback` に完全に書き込みが完了する前に、ワーカーの関数が終了し `rendition.path`ます。
+ __解像度__:カスタムワーカーコードを確認し、を使用してすべての非同期呼び出しが同期されていることを確認しま `await`す。

## 開発ツール{#development-tool}

### asset computeプロジェクト{#missing-console-json}にConsole.jsonファイルがありません

+ __エラー：__ エラー：検証時に必要なファイルが見つかりません(.../node_modules/@adobe/asset-compute-client/lib/integrationConfiguration.js:XX:YY)を非同期setupAssetCompute (.../node_modules/@adobe/asset-compute-devtool/src/assetComputeDevTool.js:XX:YY)
+ __原因：__ ファイルが `console.json` Asset computeプロジェクトのルートにありません
+ __解決方法：__ Adobe I/Oプロジェクトから新しいフ `console.json` ォームをダウンロードしてください
   1. console.adobe.ioで、Asset computeプロジェクトが使用するように設定されているAdobe I/Oプロジェクトを開きます。
   1. 右上の「__ダウンロード__」ボタンをタップします
   1. ファイル名`console.json`を使用して、ダウンロードしたファイルをAsset computeプロジェクトのルートに保存します。

### manifest.yml{#incorrect-yaml-indentation}のYAMLインデントが正しくありません

+ __エラー：__ YAMLException:行X、列Y:（標準out fromコマンドを使用）のマッピングエントリの不正なインデ `aio app run` ント
+ __原因：__ Yamlファイルはホワイトスペースに依存しているので、インデントが正しくない可能性があります。
+ __解決方法：__ を確認し、す `manifest.yml` べてのインデントが正しいことを確認します。

### memorySizeの上限が小さすぎます{#memorysize-limit-is-set-too-low}

+ __エラー：__  ローカル開発サーバーOpenWhiskError:PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=trueがHTTP 400（不正なリクエスト）を返しました —> 「リクエストの内容が正しくありません：requirementが失敗しました：メモリ64 MBが許容しきい値134217728 Bを下回る
+ __原因：__ 内のワーカ `memorySize` ーの制限が、エラーメッセージで報告される最小許容しきい値 `manifest.yml` （バイト単位）を下回って設定されました。
+ __解決方法：__  の制限を確 `memorySize` 認 `manifest.yml` し、すべて許可された最小しきい値を超えていることを確認します。

### private.key{#missing-private-key}が見つからないため、開発ツールを起動できません

+ __エラー：__ ローカル開発サーバーエラー：validatePrivateKeyFileに必要なファイルがありません….（`aio app run`コマンドから標準出力）
+ __原因：__ ファイル内 `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` の `.env` 値が、現在のユーザーに `private.key` よ `private.key` って読み取り不可を指していない。
+ __解決方法：__ ファイル内の `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 値を確認 `.env` し、ファイルシステム上のへの完全な絶対パスが含まれてい `private.key` ることを確認します。

### ソースファイルのドロップダウンが正しくない{#source-files-dropdown-incorrect}

asset compute開発ツールは、古いデータを取り込む状態に入る場合があり、「__ソースファイル__」ドロップダウンに間違った項目が表示されるのが最も顕著です。

+ __エラー：__ ソースファイルドロップダウンに誤った項目が表示される。
+ __原因：__ キャッシュされたブラウザーの状態が古いと、
+ __解決方法：__ ブラウザーで、ブラウザータブの「アプリケーション状態」、ブラウザーキャッシュ、ローカルストレージ、サービスワーカーを完全にクリアします。

### devToolTokenクエリパラメーター{#missing-or-invalid-devtooltoken-query-parameter}が見つからないか、無効です

+ __エラー：__ Asset compute開発ツールの「未承認」通知
+ __原因：__ `devToolToken` が見つからないか無効です
+ __解決方法：__ Asset compute開発ツールブラウザーウィンドウを閉じ、コマンドを使用して開始した実行中の開発ツールプロセスを終了 `aio app run` し、（を使用して）開発ツールを再起動しま `aio app run`す。

### ソースファイル{#unable-to-remove-source-files}を削除できません

+ __エラー：__ 開発ツールUIから追加されたソースファイルを削除する方法はありません
+ __原因：__ この機能は実装されていません
+ __解決方法：__ で定義された資格情報を使用して、クラウドストレージプロバイダーにログインしま `.env`す。開発ツールで使用されるコンテナ（`.env`でも指定）を探し、__source__&#x200B;フォルダーに移動して、ソース画像を削除します。 削除されたソースファイルが開発ツールの「アプリケーションの状態」でローカルにキャッシュされる可能性があるので、ドロップダウンに引き続き表示される場合は、[「ソースファイル」ドロップダウンに記載されている手順を実行する必要があります。](#source-files-dropdown-incorrect)

   ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## テスト{#test}

### テストの実行中にレンディションが生成されません。{#test-no-rendition-generated}

+ __エラー：__ エラー：レンディションが生成されません。
+ __原因：__ JavaScript構文エラーなどの予期しないエラーが原因で、ワーカーがレンディションを生成できませんでした。
+ __解決方法：__ でテスト実行を確認し `test.log` ま `/build/test-results/test-worker/test.log`す。このファイル内で、失敗したテストケースに対応するセクションを探し、エラーを確認します。

   ![トラブルシューティング — レンディションが生成されない](./assets/troubleshooting/test__no-rendition-generated.png)

### テストで誤ったレンディションが生成され、テストが失敗する{#tests-generates-incorrect-rendition}

+ __エラー：__ エラー：レンディション「rendition.xxx」が期待どおりではありません。
+ __原因：__ ワーカーは、テストケースで指定されたと異なるレンデ `rendition.<extension>` ィションを出力します。
   + 想定される`rendition.<extension>`ファイルが、テストケースでローカルに生成されたレンディションとまったく同じ方法で作成されない場合、ビットに何らかの違いがあるので、テストが失敗する可能性があります。 例えば、Asset computeワーカーがAPIを使用してコントラストを変更し、期待される結果がAdobe Photoshop CCでコントラストを調整して作成された場合、ファイルは同じように見えますが、ビットの小さなバリエーションが異なる場合があります。
+ __解決方法：__ に移動してテストからのレンディション出力を確認 `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`し、テストケースで期待されるレンディションファイルと比較します。正確に予測されるアセットを作成するには、次のいずれかを実行します。
   + 開発ツールを使用して、レンディションを生成し、正しいことを検証し、それを期待されるレンディションファイルとして使用します
   + または、`/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`でテスト生成ファイルを検証し、正しいことを検証して、それを期待されるレンディションファイルとして使用します

## デバッグ

### Debuggerは{#debugger-does-not-attach}をアタッチしません

+ __エラー__:起動の処理中にエラーが発生しました：エラー：デバッグターゲットに接続できませんでした…
+ __原因__:Docker Desktopがローカルシステムで動作していません。VS Codeデバッグコンソール（表示/デバッグコンソール）を確認して、このエラーが報告されていることを確認します。
+ __解像度__:Docker Desktopを起 [動し、必要なDockerイメージがインストールされていることを確認します](./set-up/development-environment.md#docker)。

### ブレークポイントが一時停止しない{#breakpoints-no-pausing}

+ __エラー__:デバッグ可能な開発ツールからAsset computeワーカーを実行する場合、VS Codeはブレークポイントで一時停止しません。

#### VS Codeデバッガが接続されていません{#vs-code-debugger-not-attached}

+ __原因：__ VS Codeデバッガーが停止したか、接続が切断されました。
+ __解決方法：__ VS Codeデバッガーを再起動し、VS Codeデバッグ出力コンソール（表示/デバッグコンソール）を見て、デバッガーが接続されていることを確認します。

#### ワーカーの実行開始後に添付されるVS Codeデバッガ{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__  Runin Development Toolをタップする前に、VS Codeデバッガーがア ____ タッチされませんでした。
+ __解決方法：__ VS Codeのデバッグコンソール（表示/デバッグコンソール）を確認し、開発ツールからAsset computeワーカーを再実行して、デバッガーが接続されていることを確認します。

### {#worker-times-out-while-debugging}のデバッグ中にワーカーがタイムアウトする

+ __エラー__:デバッグコンソールが「 —XXXミリ秒でアクションがタイムアウトする」、または [Asset compute開発ツールのレンディションプ](./develop/development-tool.md) レビューが無期限または無期限にスピンする
+ __原因__:デバッグ中にmanifest.ymlisで定義されたワーカ [ーのタイムア](./develop/manifest.md) ウトを超えました。
+ __解像度__:manifest.ymlorでワーカーのタイムアウトを一時的に増やすと、デ [バッグアクティビティが](./develop/manifest.md) 高速化します。

### デバッガプロセス{#cannot-terminate-debugger-process}を終了できません

+ __エラー__: `Ctrl-C` コマンドラインで、デバッガープロセスを終了しない(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3.xのバ `@adobe/aio-cli-plugin-asset-compute` グにより、終了コマンドとし `Ctrl-C` て認識されなくなります。
+ __解像度__:バー `@adobe/aio-cli-plugin-asset-compute` ジョン1.4.1以降に更新

   ```
   $ aio update
   ```

   ![トラブルシューティング — aioの更新](./assets/troubleshooting/debug__terminate.png)

## デプロイ{#deploy}

### AEM{#custom-rendition-missing-from-asset}のアセットにカスタムレンディションが見つからない

+ __エラー：__ 新しい再処理されたアセットは正常に処理されましたが、カスタムレンディションが見つかりません

#### 上位フォルダーに処理プロファイルが適用されていません

+ __原因：__ カスタムワーカーを使用する処理プロファイルを持つフォルダーの下にアセットが存在しません
+ __解決方法：__ アセットの上位フォルダーへの処理プロファイルの適用

#### 低い処理プロファイルに置き換えられた処理プロファイル

+ __原因：__ カスタムワーカーの処理プロファイルが適用されたフォルダーの下にアセットが存在しますが、そのフォルダーとアセットの間に、カスタマーワーカーを使用しない別の処理プロファイルが適用されています。
+ __解決方法：__ 2つの処理プロファイルを組み合わせるか、調整して、中間の処理プロファイルを削除します

### AEM{#asset-processing-fails}でのアセット処理が失敗する

+ __エラー：__ アセット処理に失敗したバッジがアセットに表示される
+ __原因：__ カスタムワーカーの実行中にエラーが発生しました
+ __解決方法：__ を使用したAdobe I/O Runtimeのアクティベーションのデ [バッグ手順に](./test-debug/debug.md#aio-app-logs) 従ってく `aio app logs`ださい。


