---
title: 第3章 —イベントコンテンツフラグメントのオーサリング — コンテンツサービス
seo-title: AEM Content Services使用の手引き — 第3章 —イベントコンテンツのフラグメントのオーサリング
description: AEMヘッドレスチュートリアルの第3章では、第2章で作成したコンテンツフラグメントモデルから、イベントコンテンツフラグメントを作成し、オーサリングする方法について説明します。
seo-description: AEMヘッドレスチュートリアルの第3章では、第2章で作成したコンテンツフラグメントモデルから、イベントコンテンツフラグメントを作成し、オーサリングする方法について説明します。
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 3%

---


# 第3章 —イベントコンテンツフラグメントのオーサリング

AEMヘッドレスチュートリアルの第3章では、[第2章](./chapter-2.md)で作成したコンテンツフラグメントモデルから、イベントのコンテンツフラグメントを作成およびオーサリングする方法について説明します。

## イベントコンテンツフラグメントのオーサリング

[!DNL Event]コンテンツフラグメントモデルを作成し、WKNDのAEM設定を`/content/dam/wknd-mobile`アセットフォルダーに（`cq:conf`プロパティを介して）適用すると、[!DNL Event]コンテンツフラグメントを作成できます。

アセットの一種であるコンテンツフラグメントは、他のアセットと同様に、AEM Assetsで整理、管理する必要があります。

* 変換が必要な場合（または必要な場合）は、Assetsフォルダー構造のロケールフォルダーを使用します。
* コンテンツフラグメントを論理的に整理し、見つけやすく管理しやすくする

この手順では、`/content/dam/wknd-mobile/en/events`アセットフォルダーに`Punkrock Fest`用の新しい[!DNL Event]を作成します。

1. **[!UICONTROL AEM] > [!UICONTROL アセット] > [!UICONTROL ファイル] > [!DNL WKND Mobile] >[!DNL English]**&#x200B;に移動し、アセットフォルダー&#x200B;**[!DNL Events]**&#x200B;を作成します。
1. **[!UICONTROL アセット] > [!UICONTROL ファイル] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**&#x200B;内に、タイトル&#x200B;**[!DNL Event]**&#x200B;の新しいコンテンツフラグメントを&#x200B;**[!DNL Punkrock Fest]**&#x200B;で作成します。
1. 新しく作成された[!DNL Event]コンテンツフラグメントを作成します。

   * [!DNL Event Title]：**[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **音楽**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **ザレプティルハウス**
   * [!DNL Venue City] : **ニューヨーク**

   上部のアクションバーにある「**[!UICONTROL 保存]**」をタップして、変更を保存します。

1. [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用して、AEM Authorに次のパッケージをインストールします。 このパッケージには、多数のイベントコンテンツフラグメントが含まれています。

   [ファイルの取得：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## コンテンツフラグメントのJCR構造の確認

*この節は情報提供のみを目的としており、コンテンツフラグメントモデルから作成されるコンテンツフラグメントの基盤となるJCR構造を関連付けることを目的としています。*

1. AEM作成者の&#x200B;**[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**&#x200B;を開きます。
1. CRXDE Liteの左側の階層メニューで、JCRの[!DNL Punkrock Fest] [!DNL Event]コンテンツフラグメントを表すイベントである[/content/dam/wknd-mobile/en/noperties/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)に移動します。
1. [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)ノードを展開します。
**プロパティウィンドウ**&#x200B;で、[!DNL Event]コンテンツフラグメントモデル定義を指すプロパティ`cq:model`があることを確認します。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. `data`ノードの下で、[master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)ノードを選択し、プロパティを確認します。 このノードには、[!DNL Event]コンテンツフラグメントモデルのオーサリング中に収集されたコンテンツが含まれます。 JCRプロパティ名はコンテンツフラグメントモデルのプロパティ名に対応し、値は「[!DNL Punkrock Fest]」 [!DNL Event]コンテンツフラグメントの作成済み値に対応します。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 次の手順

[AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp)を介して、AEM Authorに[com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージをインストールすることをお勧めします。 このパッケージには、チュートリアルのこの章と前の章で概要を説明している設定と内容が含まれています。

* [第4章 — AEM Content Servicesテンプレートの定義](./chapter-4.md)
