---
title: asset computeワーカーとAEM処理プロファイルの統合
description: AEM as a Cloud Serviceは、AEM Assets処理プロファイルを介してAdobe I/O RuntimeにデプロイされるAsset computeワーカーと統合されます。 処理プロファイルは、カスタムワーカーを使用して特定のアセットを処理するようにAuthorサービスで設定され、アセットレンディションとしてワーカーによって生成されたファイルを保存します。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 2%

---


# AEM処理プロファイルとの統合

asset computeワーカーがCloud ServiceとしてAEMでカスタムレンディションを生成するには、処理プロファイルを介してAEM as aCloud Serviceオーサーサービスとして登録する必要があります。 その処理プロファイルの対象となるすべてのアセットには、アップロードまたは再処理時にワーカーが呼び出され、カスタムレンディションが生成され、アセットのレンディションを介して使用できるようになります。

## 処理プロファイルの定義

最初に、設定可能なパラメーターを使用してワーカーを呼び出す新しい処理プロファイルを作成します。

![処理プロファイル](./assets/processing-profiles/new-processing-profile.png)

1. __AEM管理者__&#x200B;としてAEM as a Cloud Serviceオーサーサービスにログインします。 これはチュートリアルなので、開発環境またはサンドボックス内の環境を使用することをお勧めします。
1. __ツール/アセット/処理プロファイル__&#x200B;に移動します。
1. 「__作成__」ボタンをタップします
1. 処理プロファイルに`WKND Asset Renditions`と名前を付けます。
1. 「__カスタム__」タブをタップし、「__新規追加__」をタップします。
1. 新しいサービスの定義
   + __レンディション名:__ `Circle`
      + AEM Assetsでこのレンディションを識別するために使用されるファイル名レンディション
   + __拡張子：__ `png`
      + 生成されるレンディションの拡張。 `png`に設定します。これは、ワーカーのWebサービスでサポートされる出力形式で、円の切り取りの背後に透明な背景が表示されます。
   + __エンドポイント：__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + これは、`aio app get-url`を介して取得したワーカーへのURLです。 URLが、AEM as a Workspace環境に基づいて正しいCloud Serviceを指していることを確認します。
      + ワーカーURLが正しいワークスペースを指していることを確認します。 AEM as a Cloud ServiceステージではステージワークスペースのURLを使用し、AEM as aCloud Service実稼動環境では実稼動ワークスペースのURLを使用する必要があります。
   + __サービスパラメーター__
      + 「__パラメーターを追加__」をタップします。
         + キー: `size`
         + 値：`1000`
      + 「__パラメーターを追加__」をタップします。
         + キー: `contrast`
         + 値：`0.25`
      + 「__パラメーターを追加__」をタップします。
         + キー: `brightness`
         + 値：`0.10`
      + これらのキーと値のペアは、Asset computeワーカーに渡され、`rendition.instructions` JavaScriptオブジェクトを介して使用できます。
   + __MIME タイプ__
      + __次を含みます。__ `image/jpeg`、 `image/png`、 `image/gif`、 `image/bmp`  `image/tiff`
         + ワーカーのnpmモジュールは、これらのMIMEタイプのみです。 このリストは、カスタムワーカーが処理するアセットを制限します。
      + __除外：__ `Leave blank`
         + このサービス設定を使用して、これらのMIMEタイプを持つアセットを決して処理しないでください。 この場合、使用するのは許可リストのみです。
1. 右上の「__保存__」をタップします

## 処理プロファイルの適用と呼び出し

1. 新しく作成した処理プロファイル`WKND Asset Renditions`を選択します。
1. 上部のアクションバーで「__プロファイルをフォルダーに適用__」をタップします
1. 処理プロファイルを適用するフォルダー（`WKND`など）を選択し、「__適用__」をタップします
1. __AEM/アセット/ファイル__&#x200B;経由で処理プロファイルが適用されなかったフォルダーに移動し、`WKND`をタップします。
1. 処理プロファイルを適用したフォルダーの下の任意のフォルダーに新しい画像アセット（[sample-1.jpg](../assets/samples/sample-1.jpg)、[sample-2.jpg](../assets/samples/sample-2.jpg)および[sample-3.jpg](../assets/samples/sample-3.jpg)）をアップロードし、アップロードされたアセットの処理を待ちます。
1. アセットをタップして詳細を開きます
   + デフォルトのレンディションは、カスタムのレンディションよりもAEMで生成され、より迅速に表示される場合があります。
1. 左側のサイドバーから&#x200B;__レンディション__&#x200B;ビューを開きます
1. `Circle.png`という名前のアセットをタップし、生成されたレンディションを確認します

   ![生成されたレンディション](./assets/processing-profiles/rendition.png)

## 完了!

バリデーターがAEM as aCloud ServiceAsset computeマイクロサービスの拡張方法に関する[チュートリアル](../overview.md)を完了しました。 AEM as a Cloud Service as a Authorサービスで使用するカスタムAsset computeワーカーを設定、開発、テスト、デバッグ、デプロイする機能が備わっています。

### Githubでプロジェクトのソースコード全体を確認する

最終的なAsset computeプロジェクトは、GitHubで次の場所から入手できます。

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_「次を含む」GitHubには、ワーカーとテストケースが完全に入力されたプロジェクトの最終状態が表示されますが、資格情報は含まれていません(つまり、`.env`, `.config.json` または `.aio`._

## トラブルシューティング

+ [AEMのアセットにカスタムレンディションがない](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [AEMでのアセット処理の失敗](../troubleshooting.md#asset-processing-fails)
