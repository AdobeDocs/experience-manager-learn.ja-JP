---
title: asset computeワーカーとAEM処理プロファイルの統合
description: AEM as a Cloud Serviceは、AEM Assets処理プロファイルを介してAdobe I/O RuntimeにデプロイされるAsset computeワーカーと統合されます。 処理プロファイルは、カスタムワーカーを使用して特定のアセットを処理するように Author サービスで設定され、アセットレンディションとしてワーカーによって生成されたファイルを保存します。
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 179
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 13%

---

# AEM 処理プロファイルとの統合

asset computeワーカーがAEM as a Cloud Serviceでカスタムレンディションを生成するには、処理プロファイルを介してAEMas a Cloud Serviceのオーサーサービスに登録する必要があります。 その処理プロファイルの対象となるすべてのアセットには、アップロード時または再処理時にワーカーが呼び出され、カスタムレンディションが生成されて、アセットのレンディションを介して使用できるようになります。

## 処理プロファイルの定義

最初に、設定可能なパラメーターを使用してワーカーを呼び出す新しい処理プロファイルを作成します。

![処理プロファイル](./assets/processing-profiles/new-processing-profile.png)

1. AEM as a Cloud Service Author サービスに as a としてログイン __AEM Administrator__. これは、サンドボックス内で開発環境または環境を使用することをお勧めします。
1. __ツール／アセット／処理プロファイル__&#x200B;に移動します。
1. タップ __作成__ ボタン
1. 処理プロファイルに名前を付けます。 `WKND Asset Renditions`
1. 「__カスタム__」タブをタップし、「__新規追加__」をタップします
1. 新しいサービスを定義します
   + __レンディション名：__ `Circle`
      + AEM Assetsでこのレンディションを識別するために使用されたレンディションのファイル名
   + __拡張子：__ `png`
      + 生成されるレンディションの拡張。 をに設定します。 `png` これは、作業者の web サービスがサポートするサポートされる出力形式で、円の切り取りの背後に透明な背景が作成されるためです。
   + __エンドポイント：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + これは、を介して取得されたワーカーへの URL です。 `aio app get-url`. URL が、AEM as a Cloud Service 環境に基づいて正しいワークスペースを指していることを確認します。
      + ワーカー URL が正しいワークスペースを指していることを確認します。 AEMas a Cloud Serviceステージではステージワークスペースの URL を使用し、AEMas a Cloud Service実稼動では実稼動ワークスペースの URL を使用する必要があります。
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
      + これらのキーと値のペアは、Asset computeワーカーに渡され、を通じて使用できます。 `rendition.instructions` JavaScript オブジェクト。
   + __MIME タイプ__
      + __次を含む：__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + ワーカーの npm モジュールで使用される MIME タイプは、これらの MIME タイプのみです。 このリストは、カスタムワーカーが処理する処理を制限します。
      + __除外：__ `Leave blank`
         + このサービス設定を使用して、これらの MIME タイプを持つアセットを決して処理しないでください。 この場合、使用するのは許可リストのみです。
1. 右上の「__保存__」をタップ

## 処理プロファイルの適用と呼び出し

1. 新しく作成した処理プロファイルを選択します。 `WKND Asset Renditions`
1. タップ __プロファイルをフォルダーに適用__ 上部のアクションバー
1. 処理プロファイルを適用するフォルダー（例： ）を選択します。 `WKND` とタップします。 __適用__
1. を介して処理プロファイルが適用されなかったフォルダーに移動します。 __AEM / Assets /ファイル__ をタップし、 `WKND`.
1. 新しい画像アセット ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg)、および [sample-3.jpg](../assets/samples/sample-3.jpg)) は、処理プロファイルが適用されたフォルダーの下にある任意のフォルダーで、アップロードされたアセットの処理待ちです。
1. アセットをタップして詳細を開きます
   + デフォルトのレンディションは、カスタムレンディションよりもAEMで迅速に生成および表示される場合があります。
1. を開きます。 __レンディション__ 左側のサイドバーから表示
1. 次の名前のアセットをタップします： `Circle.png` 生成されたレンディションを確認します。

   ![生成されたレンディション](./assets/processing-profiles/rendition.png)

## 終了！

これで完了です。完了しました [チュートリアル](../overview.md) AEMas a Cloud ServiceのAsset computeマイクロサービスを拡張する方法について これで、AEMas a Cloud Serviceのオーサーサービスで使用するカスタムAsset computeワーカーを設定、開発、テスト、デバッグおよびデプロイする機能が必要になりました。

### Github でプロジェクトのソースコード全体を確認する

最終的なAsset computeプロジェクトは、GitHub で次の場所から入手できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_「次を含む」GitHub はプロジェクトの最終状態で、ワーカーとテストケースが完全に入力されますが、資格情報 ( ) は含まれていません。 `.env`, `.config.json` または `.aio`._

## トラブルシューティング

+ [AEM のアセットにカスタムレンディションがない](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEM でのアセット処理が失敗する](../troubleshooting.md#asset-processing-fails)
