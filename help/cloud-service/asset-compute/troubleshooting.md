---
title: AEM Assetsのアセット計算の拡張機能のトラブルシューティング
description: 以下は、AEM Assets向けのカスタムアセットコンピューティングワーカーの開発および展開時に発生する可能性がある、一般的な問題とエラー、および解決策のインデックスです。
feature: asset-compute
topics: renditions, metadata, development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5802
thumbnail: KT-5802.jpg
translation-type: tm+mt
source-git-commit: 6f5df098e2e68a78efc908c054f9d07fcf22a372
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 0%

---


# Asset Computeの拡張機能のトラブルシューティング

以下は、AEM Assets向けのカスタムアセットコンピューティングワーカーの開発および展開時に発生する可能性がある、一般的な問題とエラー、および解決策のインデックスです。

## 開発{#develop}

### レンディションが部分的に描画または破損して返される{#rendition-returned-partially-drawn-or-corrupt}

+ __エラー__:レンディションが完全にレンダリングされない（画像が破損している場合）、または開くことができない。

   ![レンディションが部分的に描画されて返される](./assets/troubleshooting/develop__await.png)

+ __原因__:レンディションの書き込みが完了する前に、ワーカーの `renditionCallback` 機能を終了してい `rendition.path`ます。
+ __解像度__:カスタムワーカーコードを確認し、を使用してすべての非同期呼び出しが同期されることを確認 `await`します。

## 開発ツール{#development-tool}

### manifest.ymlのYMLインデントが正しくありません{#incorrect-yaml-indentation}

+ __エラー：__ YAMLException:行X、列Y:(標準out from `aio app run` コマンドを使用)のマッピングエントリのインデントが正しくありません
+ __原因：__ Yamlファイルは空白が区別されます。インデントが正しくない可能性があります。
+ __解像度：__ を確認し `manifest.yml` 、すべてのインデントが正しいことを確認します。

### memorySize制限が低すぎます{#memorysize-limit-is-set-too-low}

+ __エラー：__ ローカルデベロッパーサーバーOpenWiskError:PUThttps://adobeioruntime.net/api/v1/namespaces/xxx-xxx-xxx/actions/xxx-0.0.1/__secured_workeroverwrite=trueがHTTP 400を返しました（無効な要求） —> &quot;要求内容が正しくありません：要件が失敗しました：メモリ64 MBが許容しきい値134217728 Bインチを下回っています。
+ __原因：__ 内のワーカーの `memorySize``manifest.yml` 制限が、エラーメッセージで報告される最小許容しきい値（バイト単位）を下回るように設定されました。
+ __解像度：__ の `memorySize` 制限を確認し、すべて許容され `manifest.yml` る最小しきい値より大きいことを確認します。

### private.keyが見つからないため、開発ツールは開始できません{#missing-private-key}

+ __エラー：__ ローカル開発サーバーエラー：validatePrivateKeyFileに必要なファイルがありません…. (標準出力 `aio app run` コマンドを使用)
+ __原因：__ フ `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` ァイル内の `.env` 値が、現在のユーザーが参照し `private.key``private.key` ていない、または読み取りできない。
+ __解像度：__ ファイル内の `ASSET_COMPUTE_PRIVATE_KEY_FILE_PATH` 値を確認し、ファイルシステム `.env``private.key` 上ののの完全で絶対パスが含まれていることを確認します。

### ソースファイルのドロップダウンが正しくない{#source-files-dropdown-incorrect}

アセット計算開発ツールは、古いデータを取り込む状態に入る場合があり、 __ソースファイル__ (Source File)ドロップダウンに誤った項目が表示されるのが最も顕著になります。

+ __エラー：__ ソースファイルのドロップダウンに正しくない項目が表示される。
+ __原因：__ キャッシュされたブラウザーの状態が古い場合、
+ __解像度：__ ブラウザーで、ブラウザータブの「アプリケーション状態」、ブラウザーのキャッシュ、ローカルストレージおよびサービスワーカーが完全にクリアされます。

### devToolTokenクエリパラメーターがないか、無効です{#missing-or-invalid-devtooltoken-query-parameter}

+ __エラー：__ Asset Compute Development Toolの「未認証」通知
+ __原因：__`devToolToken` が見つからないか、無効です
+ __解像度：__ 「Asset Compute Development Tool」ブラウザウィンドウを閉じ、 `aio app run` コマンドを使用して開始した実行中の開発ツールプロセスを終了し、再開始開発ツール(を使用 `aio app run`)を終了します。

### ソースファイルを削除できません{#unable-to-remove-source-files}

+ __エラー：__ 開発ツールのUIから追加したソースファイルを削除する方法はありません
+ __原因：__ この機能は実装されていません
+ __解像度：__ で定義されている資格情報を使用して、クラウドストレージプロバイダーにログインし `.env`ます。 開発ツールで使用するコンテナ(で指定 `.env`)を探し、 ____ ソースフォルダーに移動して、ソース画像を削除します。 削除したソースファイルが開発ツールの「 [アプリケーションの状態」でローカルにキャッシュされる場合があるので、ドロップダウンに引き続き表示される場合は](#source-files-dropdown-incorrect) 、「ソースファイル」のドロップダウンに記載された手順を正しく実行しないといけません。

   ![Microsoft Azure Blob Storage](./assets/troubleshooting/dev-tool__remove-source-files.png)

## テスト{#test}

### テストの実行中にレンディションが生成されませんでした{#test-no-rendition-generated}

+ __エラー：__ 失敗：レンディションが生成されません。
+ __原因：__ JavaScript構文エラーなどの予期しないエラーが発生したため、ワーカーはレンディションの生成に失敗しました。
+ __解像度：__ テストの実行を確認 `test.log` し `/build/test-results/test-worker/test.log`ます。 失敗したテストケースに対応するこのファイル内のセクションを見つけ、エラーを確認します。

   ![トラブルシューティング — レンディションが生成されません](./assets/troubleshooting/test__no-rendition-generated.png)

### テストで誤ったレンディションが生成され、テストが失敗する{#tests-generates-incorrect-rendition}

+ __エラー：__ 失敗：レンディション&#39;rendition.xxx&#39;が期待どおりではありません。
+ __原因：__ ワーカーは、テストケースで `rendition.<extension>` 指定されたレンディションと異なるレンディションを出力しました。
   + 期待される `rendition.<extension>` ファイルが、テストケースでローカルに生成されたレンディションと完全に同じ方法で作成されない場合、ビットに何らかの違いがあるので、テストが失敗する可能性があります。 例えば、アセット計算ワーカーがAPIを使用してコントラストを変更し、期待される結果がAdobe Photoshop CCでコントラストを調整して作成された場合、ファイルは同じように見えますが、ビットの小さなバリエーションは異なる場合があります。
+ __解像度：__ テストから出力されたレンディションをレビューします。その際に `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`移動し、テストケースで予想されるレンディションファイルと比較します。 正確に予想されるアセットを作成するには、次のいずれかを行います。
   + 開発ツールを使用して、レンディションを生成し、正しいことを検証し、期待どおりのレンディションファイルとして使用します
   + または、でテスト生成ファイルを検証し、正しいことを検証し `/build/test-worker/<worker-name>/<test-run-timestamp>/<test-case>/rendition.<extension>`て、期待どおりのレンディションファイルとして使用します

## デバッグ


### デバッガがアタッチしません{#debugger-does-not-attach}

+ __エラー__:起動の処理中にエラーが発生しました：エラー：次の時点でデバッグターゲットに接続できませんでした…
+ __原因__:Docker Desktopがローカルシステムで実行されていません。 VSコードデバッグコンソール(表示/デバッグコンソール)を確認して、このエラーがレポートされることを確認します。
+ __解像度__:開始 [ドッカーデスクトップで、必要なドッカーイメージがインストールされていることを確認し](./set-up/development-environment.md#docker)ます。

### ブレークポイントが一時停止されない{#breakpoints-no-pausing}

+ __エラー__:デバッグ可能な開発ツールからアセット計算ワーカーを実行する場合、VSコードはブレークポイントで一時停止しません。

#### VSコードデバッガがアタッチされていません{#vs-code-debugger-not-attached}

+ __原因：__ VSコードデバッガが停止または切断されました。
+ __解像度：__ VSコードデバッガを再起動し、VSコードデバッグ出力コンソール(表示/デバッグコンソール)を見て接続を確認します。

#### ワーカーの実行開始後に添付されたVSコードデバッガ{#vs-code-debugger-attached-after-worker-execution-began}

+ __原因：__ 「開発ツールで __実行__ 」をタップする前に、VSコードデバッガーがアタッチされませんでした。
+ __解像度：__ VSコードのデバッグコンソール(表示/デバッグコンソール)を確認し、デバッガがアタッチされていることを確認してから、開発ツールからAsset Compute workerを再実行します。

### デバッグ中に作業者がタイムアウトする{#worker-times-out-while-debugging}

+ __エラー__:Debug Consoleレポートに「Action will timeout in -XXX milliseconds」または [Asset Compute Development Toolの](./develop/development-tool.md) Renditionプレビューが無限にスピンするか、
+ __原因__:デバッグ中に [manifest.ymlで定義されているワーカーのタイムアウトを超えました](./develop/manifest.md) 。
+ __解像度__:ワーカーのタイムアウトを [manifest.ymlで一時的に増やすか](./develop/manifest.md) 、デバッグアクティビティを高速化します。

### デバッガープロセスを終了できません{#cannot-terminate-debugger-process}

+ __エラー__: `Ctrl-C` コマンドラインでデバッガプロセスが終了しない(`npx adobe-asset-compute devtool`)。
+ __原因__:1.3.xのバグ `@adobe/aio-cli-plugin-asset-compute` は、終了コマンドとして認識され `Ctrl-C` ません。
+ __解像度__:バージョン1.4.1以降 `@adobe/aio-cli-plugin-asset-compute` へのアップデート

   ```
   $ aio update
   ```

   ![トラブルシューティング — aioの更新](./assets/troubleshooting/debug__terminate.png)

## デプロイ{#deploy}

### AEMのアセットにカスタムレンディションが見つかりません{#custom-rendition-missing-from-asset}

+ __エラー：__ 新しく再処理されたアセットは正常に処理されますが、カスタムレンディションが見つかりません。

#### 処理プロファイルが上位フォルダーに適用されていません

+ __原因：__ カスタムワーカーを使用する処理プロファイルを含むフォルダーの下にアセットが存在しません
+ __解像度：__ 処理プロファイルのアセットの上位フォルダーへの適用

#### 低い処理プロファイルで置き換えられた処理プロファイル

+ __原因：__ このアセットは、カスタムワーカーの処理プロファイルが適用されたフォルダーの下に存在しますが、そのカスタマーワーカーを使用しない別の処理プロファイルが、そのフォルダーとアセットの間で適用されています。
+ __解像度：__ 2つの処理プロファイルを組み合わせて、または調整し、中間処理プロファイルを削除する

### AEMでのアセットの処理が失敗する{#asset-processing-fails}

+ __エラー：__ アセットに表示されるアセット処理に失敗したバッジ
+ __原因：__ カスタムワーカーの実行中にエラーが発生しました
+ __解像度：__ を使用したAdobe I/O Runtimeアクティベーションの [デバッグに関する手順に従い](./test-debug/debug.md#aio-app-logs)`aio app logs`ます。


