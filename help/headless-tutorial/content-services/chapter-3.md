---
title: 第 3 章 - イベントコンテンツフラグメントのオーサリング - コンテンツサービス
description: AEM ヘッドレスチュートリアルの第 3 章では、第 2 章で作成したコンテンツフラグメントモデルからイベントコンテンツフラグメントを作成およびオーサリングする方法について説明します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: 46ef11a2-81bd-4ff7-b9ef-9f8cba52c6a8
duration: 167
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 100%

---

# 第 3 章 - イベントコンテンツフラグメントのオーサリング

AEM ヘッドレスチュートリアルの第 3 章では、[第 2 章](./chapter-2.md)で作成したコンテンツフラグメントモデルからイベントコンテンツフラグメントを作成およびオーサリングする方法について説明します。

## イベントコンテンツフラグメントのオーサリング

[!DNL Event] コンテンツフラグメントモデルを作成し、WKND の AEM 設定を `/content/dam/wknd-mobile` アセットフォルダーに（`cq:conf` プロパティ経由で）適用すると、[!DNL Event] コンテンツフラグメントを作成できます。

アセットの一種であるコンテンツフラグメントは、他のアセットと同様に、AEM Assets で整理および管理する必要があります。

* 翻訳が必要な場合（または必要になる可能性がある場合）は、アセットフォルダー構造でロケールフォルダーを使用する。
* コンテンツフラグメントを論理的に整理して、見つけやすく管理しやすくする。

このステップでは、`/content/dam/wknd-mobile/en/events` アセットフォルダーに `Punkrock Fest` 用の新しい [!DNL Event] を作成します。

1. **[!UICONTROL AEM]／[!UICONTROL アセット]／[!UICONTROL ファイル]／[!DNL WKND Mobile]／[!DNL English]** に移動し、アセットフォルダー **[!DNL Events]** を作成します。
1. **[!UICONTROL アセット]／[!UICONTROL ファイル]／[!DNL WKND Mobile]／[!DNL English]／[!DNL Events]** 内に、タイプが **[!DNL Event]** でタイトルが **[!DNL Punkrock Fest]** の新しいコンテンツフラグメントを作成します。
1. 新しく作成した [!DNL Event] コンテンツフラグメントをオーサリングします。

   * [!DNL Event Title]：**[!DNL Punkrock Fest]**
   * [!DNL Event Description]：**&lt;Enter a few lines of description...>**
   * [!DNL Event Date]：**&lt;Select a date in the future>**
   * [!DNL Event Type]：**Music**
   * [!DNL Ticket Price]：**10**
   * [!DNL Event Image]：**/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name]：**The Reptile House**
   * [!DNL Venue City]：**New York**

   上部のアクションバーで「**[!UICONTROL 保存]**」をタップして、変更を保存します。

1. [AEM のパッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、以下のパッケージを AEM オーサーにインストールします。このパッケージには、多数のイベントコンテンツフラグメントが含まれています。

   [ファイルを取得：GitHub／Assets／com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338?quality=12&learn=on)

## コンテンツフラグメントの JCR 構造の確認

*この節は情報提供のみを目的としており、コンテンツフラグメントモデルから作成されたコンテンツフラグメントの基礎となる JCR 構造をソーシャル化するためのものです。*

1. AEM オーサーで **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** を開きます。
1. CRXDE Lite の左側の階層メニューで、JCR の [!DNL Punkrock Fest] [!DNL Event] コンテンツフラグメントを表すノードである [/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) に移動します。
1. [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) ノードを展開します。
[!DNL Event] コンテンツフラグメントモデル定義を指すプロパティ `cq:model` があることを、**プロパティペイン**&#x200B;で確認します。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. `data` ノードの直下の [master](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) ノードを選択し、プロパティを確認します。このノードには、[!DNL Event] コンテンツフラグメントモデルのオーサリング時に収集されたコンテンツが含まれています。JCR プロパティ名はコンテンツフラグメントモデルのプロパティ名に対応し、値は「[!DNL Punkrock Fest]」[!DNL Event] コンテンツフラグメントのオーサリングされた値に対応します。

>[!VIDEO](https://video.tv.adobe.com/v/28356?quality=12&learn=on)

## 次の手順

[AEM の[!UICONTROL パッケージマネージャー]](http://localhost:4502/crx/packmgr/index.jsp)を使用して、[com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) コンテンツパッケージを AEM オーサーにインストールすることをお勧めします。このパッケージには、チュートリアルのこの章および前の章で概要を説明した設定とコンテンツが含まれています。

* [第 4 章 - AEM コンテンツサービステンプレートの定義](./chapter-4.md)
