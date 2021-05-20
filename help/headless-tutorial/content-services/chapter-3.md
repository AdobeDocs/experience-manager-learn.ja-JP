---
title: 第3章 — オーサリングイベントコンテンツフラグメント — コンテンツサービス
seo-title: AEM Content Servicesの概要 — 第3章 — イベントコンテンツフラグメントのオーサリング
description: AEMヘッドレスチュートリアルの第3章では、第2章で作成したコンテンツフラグメントモデルからのイベントコンテンツフラグメントの作成とオーサリングについて説明します。
seo-description: AEMヘッドレスチュートリアルの第3章では、第2章で作成したコンテンツフラグメントモデルからのイベントコンテンツフラグメントの作成とオーサリングについて説明します。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 3%

---


# 第3章 — イベントコンテンツフラグメントのオーサリング

AEMヘッドレスチュートリアルの第3章では、[第2章](./chapter-2.md)で作成したコンテンツフラグメントモデルから、イベントコンテンツフラグメントを作成およびオーサリングする方法について説明します。

## イベントコンテンツフラグメントのオーサリング

[!DNL Event]コンテンツフラグメントモデルが作成され、WKND用のAEM設定が`cq:conf`プロパティを使用して`/content/dam/wknd-mobile`アセットフォルダーに適用されると、[!DNL Event]コンテンツフラグメントを作成できます。

アセットの一種であるコンテンツフラグメントは、他のアセットと同様に、AEM Assetsで整理および管理する必要があります。

* 翻訳が必要な場合は、 Assetsフォルダー構造でロケールフォルダーを使用する
* コンテンツフラグメントを論理的に整理し、見つけや管理を容易にする

この手順では、`/content/dam/wknd-mobile/en/events` assetsフォルダーに`Punkrock Fest`の新しい[!DNL Event]を作成します。

1. **[!UICONTROL AEM] / [!UICONTROL Assets] / [!UICONTROL Files] / [!DNL WKND Mobile] /[!DNL English]**&#x200B;に移動し、アセットフォルダー&#x200B;**[!DNL Events]**&#x200B;を作成します。
1. **[!UICONTROL アセット] > [!UICONTROL ファイル] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]**&#x200B;内に、タイトルが&#x200B;**[!DNL Punkrock Fest]**&#x200B;の新しいコンテンツフラグメントを作成します。**[!DNL Event]**
1. 新しく作成された[!DNL Event]コンテンツフラグメントを作成します。

   * [!DNL Event Title]：**[!DNL Punkrock Fest]**
   * [!DNL Event Description] :  **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description=&quot;&quot;>**
   * [!DNL Event Date] :  **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **音楽**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **ザ爬虫類ハウス**
   * [!DNL Venue City] : **ニューヨーク**

   上部のアクションバーの「**[!UICONTROL 保存]**」をタップして、変更を保存します。

1. [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)を使用して、AEMオーサーに以下のパッケージをインストールします。 このパッケージには、多数のイベントコンテンツフラグメントが含まれています。

   [ファイルの取得：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## コンテンツフラグメントのJCR構造の確認

*この節は情報提供だけを目的とし、コンテンツフラグメントモデルから作成されたコンテンツフラグメントの基盤となるJCR構造を関連付けるためのものです。*

1. AEMオーサーで&#x200B;**[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)**&#x200B;を開きます。
1. CRXDE Liteの左側の階層メニューで、 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content)に移動します。このノードは、JCRの[!DNL Punkrock Fest] [!DNL Event]コンテンツフラグメントを表します。
1. [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)ノードを展開します。
**プロパティウィンドウ**&#x200B;で、[!DNL Event]コンテンツフラグメントモデル定義を指すプロパティ`cq:model`があることを確認します。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. `data`ノードの下で[マスター](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master)ノードを選択し、プロパティを確認します。 このノードには、[!DNL Event]コンテンツフラグメントモデルのオーサリング中に収集されたコンテンツが含まれます。 JCRのプロパティ名はコンテンツフラグメントモデルのプロパティ名に対応し、値は「[!DNL Punkrock Fest]」 [!DNL Event]コンテンツフラグメントの作成済み値に対応します。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 次の手順

[AEM [!UICONTROL Package Manager]](http://localhost:4502/crx/packmgr/index.jsp)を使用して、AEMオーサーに[com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージをインストールすることをお勧めします。 このパッケージには、チュートリアルのこの章および前の章で概要を説明した設定とコンテンツが含まれています。

* [第4章 — AEM Content Servicesテンプレートの定義](./chapter-4.md)
