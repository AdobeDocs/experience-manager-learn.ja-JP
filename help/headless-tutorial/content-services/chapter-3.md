---
title: 第 3 章 — オーサリングイベントコンテンツフラグメント — コンテンツサービス
seo-title: Getting Started with AEM Content Services - Chapter 3 - Authoring Event Content Fragments
description: AEMヘッドレスチュートリアルの第 3 章では、第 2 章で作成したコンテンツフラグメントモデルから、イベントコンテンツフラグメントを作成およびオーサリングする方法について説明します。
seo-description: Chapter 3 of the AEM Headless tutorial covers creating and authoring Event Content Fragments from the Content Fragment Model created in Chapter 2.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 4%

---

# 第 3 章 — イベントコンテンツフラグメントのオーサリング

AEMヘッドレスチュートリアルの第 3 章では、で作成したコンテンツフラグメントモデルから、イベントのコンテンツフラグメントを作成およびオーサリングする方法について説明します。 [第 2 章](./chapter-2.md).

## イベントコンテンツフラグメントのオーサリング

を使用 [!DNL Event] コンテンツフラグメントモデルが作成され、WKND 用のAEM設定が `/content/dam/wknd-mobile` アセットフォルダー ( `cq:conf` プロパティ )、 [!DNL Event] コンテンツフラグメントを作成できます。

アセットの一種であるコンテンツフラグメントは、他のアセットと同様に、AEM Assetsで整理および管理する必要があります。

* 翻訳が必要な場合は、Assets フォルダー構造でロケールフォルダーを使用します。
* コンテンツフラグメントを論理的に整理し、見つけやすく管理しやすくします

この手順では、新しい [!DNL Event] 対象 `Punkrock Fest` 内 `/content/dam/wknd-mobile/en/events` assets フォルダー。

1. に移動します。 **[!UICONTROL AEM] > [!UICONTROL Assets] > [!UICONTROL ファイル] > [!DNL WKND Mobile] >[!DNL English]** アセットフォルダーの作成 **[!DNL Events]**.
1. 内 **[!UICONTROL Assets] > [!UICONTROL ファイル] > [!DNL WKND Mobile] > [!DNL English] >[!DNL Events]** タイプの新しいコンテンツフラグメントを作成します。 **[!DNL Event]** タイトルは **[!DNL Punkrock Fest]**.
1. 新しく作成した [!DNL Event] コンテンツフラグメント。

   * [!DNL Event Title]：**[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;enter a=&quot;&quot; few=&quot;&quot; lines=&quot;&quot; of=&quot;&quot; description...=&quot;&quot;>**
   * [!DNL Event Date] : **&lt;select a=&quot;&quot; date=&quot;&quot; in=&quot;&quot; the=&quot;&quot; future=&quot;&quot;>**
   * [!DNL Event Type] : **音楽**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **ザ爬虫類ハウス**
   * [!DNL Venue City] : **ニューヨーク**

   タップ **[!UICONTROL 保存]** をクリックして変更を保存します。

1. 使用 [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp)、AEM オーサーに次のパッケージをインストールします。 このパッケージには、多数のイベントコンテンツフラグメントが含まれています。

   [ファイルを取得：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## コンテンツフラグメントの JCR 構造のレビュー

*この節は情報提供のみを目的としており、コンテンツフラグメントモデルから作成されたコンテンツフラグメントの基盤となる JCR 構造をソーシャル化するためのものです。*

1. 開く **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** （AEM オーサー）を参照してください。
1. CRXDE Liteの左側の階層メニューで、に移動します。 [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) これは、 [!DNL Punkrock Fest] [!DNL Event] JCR 内のコンテンツフラグメント。
1. を展開します。 [データ](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) ノード。
レビュー： **プロパティペイン** プロパティを持つ `cq:model` それは [!DNL Event] コンテンツフラグメントモデルの定義。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. の `data` ノードを選択 [プライマリ](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) ノードに移動して、プロパティを確認します。 このノードには、 [!DNL Event] コンテンツフラグメントモデル。 JCR のプロパティ名はコンテンツフラグメントモデルのプロパティ名に対応し、値は「[!DNL Punkrock Fest]&quot; [!DNL Event] コンテンツフラグメント。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 次の手順

この機能を使用する場合は、 [com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) を介して AEM オーサー上のコンテンツパッケージ [AEM [!UICONTROL パッケージマネージャー]](http://localhost:4502/crx/packmgr/index.jsp). このパッケージには、チュートリアルのこの章および前の章で概要を説明する設定とコンテンツが含まれています。

* [第 4 章 — AEM Content Services テンプレートの定義](./chapter-4.md)
