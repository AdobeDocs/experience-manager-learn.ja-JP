---
title: AEM Assets Dynamic Mediaでのスマート切り抜きの使用
description: スマート切り抜きでは、Adobe Senseiを使用して、レスポンシブデザインのためにコンテンツの切り抜きにかかる時間とコストのかかるタスクを排除します。
feature: Smart Crop, Image Profiles
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 295bbfb6-241f-41c0-972d-d9688863cea1
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 17%

---

# AEM Assets Dynamic Mediaでのスマート切り抜きの使用{#using-smart-crop-with-aem-assets-dynamic-media}

スマート切り抜きでは、Adobe Senseiを使用して、レスポンシブデザインのためにコンテンツの切り抜きにかかる時間とコストのかかるタスクを排除します。

>[!VIDEO](https://video.tv.adobe.com/v/21519/)

>[!NOTE]
>
>ビデオは、AEMインスタンスがDynamic Media S7 モードで動作していることを前提としています。 [Dynamic MediaでのAEMの設定手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience ManagerのDynamic Mediaスマート切り抜き機能には、以下が含まれます。

* AEM Asset 管理者は、デバイスの幅と高さに基づいて、スマート切り抜き用のイメージプロファイルを簡単に作成できます。
* スマート切り抜きは、個々のアセットに対して実行することも、フォルダー内のすべてのアセットに対して実行することもできます。
* スマート切り抜き編集レイアウトをサイズ変更して、可視性を高めることができます。
* AEM Sites のDynamic Mediaコンポーネントは、スマート切り抜きをサポートしています。
* スマート切り抜きアセットの公開済み URL は、ホストされているアセットを受け入れるサードパーティアプリケーションで使用できます。

>[!NOTE]
>
>スマート切り抜きの座標は、縦横比に応じて異なります。 つまり、イメージプロファイルの各種スマート切り抜き設定で、イメージプロファイルに追加されたサイズの縦横比が同じ場合、同じ縦横比が Dynamic Media に送信されます。このため、スマート切り抜きエディターでは同じ切り抜き領域が提示されます。 例えば、切り抜きの設定を 100x100 と 200x200 にすると、同じスマート切り抜きがシステムによって生成されます。
