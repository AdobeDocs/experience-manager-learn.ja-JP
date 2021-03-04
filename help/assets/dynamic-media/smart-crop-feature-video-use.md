---
title: AEM AssetsDynamic Mediaでのスマートクロップの使用
description: スマート切り抜きでは、Adobe Senseiを使用して、レスポンシブデザインのために、時間とコストのかかるコンテンツの切り抜きタスクを排除します。
sub-product: dynamic-media
feature: スマート切り抜き、画像プロファイル
version: 6.4, 6.5
topic: コンテンツ管理
role: 開業医
level: 初心者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 17%

---


# AEM AssetsDynamic Mediaとのスマート切り抜きの使用{#using-smart-crop-with-aem-assets-dynamic-media}

スマート切り抜きでは、Adobe Senseiを使用して、レスポンシブデザインのために、時間とコストのかかるコンテンツの切り抜きタスクを排除します。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>ビデオでは、AEMインスタンスがDynamic MediaS7モードで実行されていることを前提としています。 [Dynamic MediaでAEMを設定する手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience ManagerのDynamic Media・スマート・クロップ機能には、

* AEM Asset Adminは、デバイスの幅と高さに基づいて、スマート切り抜き用の画像プロファイルを簡単に作成できます。
* スマート切り抜きは、個々のアセットに対して実行することも、フォルダー内のすべてのアセットに対して実行することもできます。
* 見やすいように、スマート切り抜き編集レイアウトをサイズ変更できます。
* AEMサイトのDynamic Mediaコンポーネントは、スマート切り抜きをサポートしています。
* スマートトリミングされたアセットの公開済みURLは、ホストされたアセットを受け入れるサードパーティアプリケーションで使用できます。

>[!NOTE]
>
>スマート切り抜きの座標は縦横比に依存します。 つまり、イメージプロファイルの各種スマート切り抜き設定で、イメージプロファイルに追加されたサイズの縦横比が同じ場合、同じ縦横比が Dynamic Media に送信されます。このため、スマート切り抜きエディタで同じ切り抜き領域が提示されます。 例えば、切り抜きの設定を100x100と200x200にすると、システムによって生成されるスマートな切り抜きと同じ画像が生成されます。
