---
title: AEM Assets Dynamic Mediaでのスマート切り抜きの使用
description: スマート切り抜きでは、Adobe Senseiを使用して、レスポンシブデザインのためにコンテンツを切り抜く際の時間とコストのかかるタスクを排除します。
sub-product: dynamic-media
feature: スマート切り抜き、イメージプロファイル
version: 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '253'
ht-degree: 18%

---


# AEM Assets Dynamic Mediaでのスマート切り抜きの使用{#using-smart-crop-with-aem-assets-dynamic-media}

スマート切り抜きでは、Adobe Senseiを使用して、レスポンシブデザインのためにコンテンツを切り抜く際の時間とコストのかかるタスクを排除します。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>ビデオは、AEMインスタンスがDynamic Media S7モードで動作していることを前提としています。 [Dynamic MediaでのAEMの設定手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience ManagerのDynamic Mediaスマート切り抜き機能には、

* AEM Asset管理者は、デバイスの幅と高さに基づいて、スマート切り抜き用のイメージプロファイルを簡単に作成できます。
* スマート切り抜きは、個々のアセットに対して実行することも、フォルダー内のすべてのアセットに対して実行することもできます。
* スマート切り抜き編集レイアウトは、視認性を高めるためにサイズ変更できます。
* AEM SitesのDynamic Mediaコンポーネントは、スマート切り抜きをサポートします。
* スマート切り抜きアセットの公開済みURLは、ホストされたアセットを受け入れるサードパーティアプリケーションで使用できます。

>[!NOTE]
>
>スマート切り抜きの座標は、縦横比に応じて異なります。 つまり、イメージプロファイルの各種スマート切り抜き設定で、イメージプロファイルに追加されたサイズの縦横比が同じ場合、同じ縦横比が Dynamic Media に送信されます。このため、スマート切り抜きエディターに同じ切り抜き領域が表示されます。 例えば、切り抜き設定が100x100と200x200の場合、同じスマート切り抜きがシステムで生成されます。
