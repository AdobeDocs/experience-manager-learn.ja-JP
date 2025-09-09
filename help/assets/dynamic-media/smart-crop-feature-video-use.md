---
title: AEM Assets Dynamic Media でのスマート切り抜きの使用
description: スマート切り抜きを使用すると、レスポンシブデザインでのコンテンツの切り抜きにおける時間とコストのかかるタスクを排除できます。
feature: Smart Crop, Image Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
jira: KT-784
doc-type: Feature Video
exl-id: 295bbfb6-241f-41c0-972d-d9688863cea1
duration: 443
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 87%

---

# AEM Assets Dynamic Media でのスマート切り抜きの使用{#using-smart-crop-with-aem-assets-dynamic-media}

スマート切り抜きを使用すると、レスポンシブデザインでのコンテンツの切り抜きにおける時間とコストのかかるタスクを排除できます。

>[!VIDEO](https://video.tv.adobe.com/v/40412?quality=12&learn=on&captions=jpn)

>[!NOTE]
>
>ビデオでは、AEM インスタンスが Dynamic Media S7 モードで動作していることを前提としています。[Dynamic Media での AEM の設定手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## Adobe Experience Manager の Dynamic Media スマート切り抜き機能には、以下が含まれます。

* AEM Asset 管理者は、デバイスの幅と高さに基づいて、スマート切り抜き用のイメージプロファイルを簡単に作成できます。
* スマート切り抜きは、個々のアセットに対して実行することも、フォルダー内のすべてのアセットに対して実行することもできます。
* スマート切り抜き編集レイアウトでは、サイズ変更して可視性を高めることができます。
* AEM Sites の Dynamic Media コンポーネントは、スマート切り抜きをサポートしています。
* スマート切り抜きアセットの公開済み URL は、ホストされているアセットを受け入れるサードパーティアプリケーションで使用できます。

>[!NOTE]
>
>スマート切り抜きの座標は、アスペクト比に応じて異なります。 つまり、イメージプロファイルの各種スマート切り抜き設定で、イメージプロファイルに追加されたサイズの縦横比が同じ場合、同じ縦横比が Dynamic Media に送信されます。このため、スマート切り抜きエディターでは同じ切り抜き領域が提示されます。 例えば、切り抜きの設定を 100x100 と 200x200 にすると、同じスマート切り抜きがシステムによって生成されます。
