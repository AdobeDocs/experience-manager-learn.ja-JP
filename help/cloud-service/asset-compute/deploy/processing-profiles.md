---
title: AEM 処理プロファイルを通じた Asset Compute ワーカーの統合
description: AEM as a Cloud Service は、AEM Assets 処理プロファイルを介して Adobe I/O Runtime にデプロイされる Asset Compute ワーカーと統合されます。処理プロファイルは、カスタムワーカーを使用して特定のアセットを処理するようにオーサーサービスで設定され、ワーカーがアセットレンディションとして生成したファイルを保存します。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 168
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '622'
ht-degree: 100%

---

# AEM 処理プロファイルとの統合

Asset Compute ワーカーが AEM as a Cloud Service でカスタムレンディションを生成するには、処理プロファイルを介して AEM as a Cloud Service のオーサーサービスに登録する必要があります。その処理プロファイルの対象となるすべてのアセットは、アップロード時または再処理時にワーカーを呼び出し、カスタムレンディションを生成して、アセットのレンディションを介して使用できるようにします。

## 処理プロファイルの作成

最初に、設定可能なパラメーターを使用してワーカーを呼び出す新しい処理プロファイルを作成します。

![処理プロファイル](./assets/processing-profiles/new-processing-profile.png)

1. AEM as a Cloud Service オーサーサービスに __AEM 管理者__&#x200B;としてログインします。これはチュートリアルなので、開発環境またはサンドボックス内の環境を使用することをお勧めします。
1. __ツール／アセット／処理プロファイル__&#x200B;に移動します。
1. 「__作成__」ボタンをタップします。
1. 処理プロファイルに名前を付けます。`WKND Asset Renditions`
1. 「__カスタム__」タブをタップし、「__新規追加__」をタップします
1. 新しいサービスを定義します
   + __レンディション名：__`Circle`
      + AEM Assets でこのレンディションを識別するために使用されたレンディションのファイル名
   + __拡張子：__`png`
      + 生成されるレンディションの拡張子。 `png`（ワーカーの Web サービスがサポートする出力形式）に設定します。円形の切り抜きの背後に透明な背景が表示されます。
   + __エンドポイント：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + これは、`aio app get-url` を介して取得されたワーカーへの URL です。URL が、AEM as a Cloud Service 環境に基づいて正しいワークスペースを指していることを確認します。
      + ワーカー URL が正しいワークスペースを指していることを確認します。AEM as a Cloud Service Stage はステージワークスペース URL を使用し、AEM as a Cloud Service Production は本番ワークスペース URL を使用する必要があります。
   + __サービスパラメーター__
      + 「__パラメーターを追加__」をタップします
         + キー：`size`
         + 値：`1000`
      + 「__パラメーターを追加__」をタップします
         + キー：`contrast`
         + 値：`0.25`
      + 「__パラメーターを追加__」をタップします
         + キー：`brightness`
         + 値：`0.10`
      + これらのキーと値のペアは、Asset Compute ワーカーに渡され、`rendition.instructions` JavaScript オブジェクトを通じて使用できます。
   + __MIME タイプ__
      + __次を含む：__ `image/jpeg`、`image/png`、`image/gif`、`image/bmp``image/tiff`
         + ワーカーの npm モジュールで使用される MIME タイプは、これらの MIME タイプのみです。 このリストは、カスタムワーカーが処理する処理を制限します。
      + __除外：__ `Leave blank`
         + このサービス設定を使用して、これらの MIME タイプを持つアセットを決して処理しないでください。 この場合、使用するのは許可リストのみです。
1. 右上の「__保存__」をタップ

## 処理プロファイルの適用と呼び出し

1. 新規作成された処理プロファイルを選択します。`WKND Asset Renditions`
1. 上部のアクションバーで「__フォルダーにプロファイルを適用__」をタップします。
1. 処理プロファイルを適用するフォルダー（例：`WKND`）を選択して、「__適用__」をタップします。
1. __AEM／Assets／ファイル__、`WKND` の順にタップして、処理プロファイルが適用されなかったフォルダーに移動します。
1. 新しい画像アセット（[sample-1.jpg](../assets/samples/sample-1.jpg)、[sample-2.jpg](../assets/samples/sample-2.jpg)、[sample-3.jpg](../assets/samples/sample-3.jpg)）をアップロードします。処理プロファイルが適用されたフォルダーの下の任意のフォルダーを選択し、アップロードされたアセットが処理されるまで待ちます。
1. 詳細を表示するにはアセットをタップします。
   + デフォルトのレンディションは、カスタムレンディションよりも AEM で迅速に生成および表示される場合があります。
1. 左側のサイドバーから&#x200B;__レンディション__&#x200B;ビューを開きます。
1. `Circle.png` という名前のアセットをタップして、生成されたレンディションを確認します。

   ![生成されたレンディション](./assets/processing-profiles/rendition.png)

## 完了です。

これですべて完了です。AEM as a Cloud Service Asset Computee マイクロサービスを拡張する方法に関する[チュートリアル](../overview.md)を完了しました。これで、AEM as a Cloud Service Author サービスで使用するカスタム Asset Compute ワーカーを設定、開発、テスト、デバッグ、デプロイできるようになりました。

### Github でプロジェクトのソースコード全体を確認する

最終的な Asset Computeプロジェクトは、GitHub の次の場所から入手できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub にはプロジェクトの最終状態が含まれ、ワーカーとテストケースが完全に入力されますが、資格情報（`.env`、`.config.json` または `.aio`_）は含まれません。

## トラブルシューティング

+ [AEM のアセットにカスタムレンディションがない](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM でのアセット処理が失敗する](../troubleshooting.md#asset-processing-fails)
