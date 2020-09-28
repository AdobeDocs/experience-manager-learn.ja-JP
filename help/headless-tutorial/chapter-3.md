---
title: 第3章 —イベントコンテンツフラグメントのオーサリング
seo-title: AEM Content Services使用の手引き — 第3章 —イベントコンテンツのフラグメントのオーサリング
description: AEMヘッドレスチュートリアルの第3章では、第2章で作成したコンテンツフラグメントモデルから、イベントコンテンツフラグメントを作成し、オーサリングする方法について説明します。
seo-description: AEMヘッドレスチュートリアルの第3章では、第2章で作成したコンテンツフラグメントモデルから、イベントコンテンツフラグメントを作成し、オーサリングする方法について説明します。
translation-type: tm+mt
source-git-commit: d7258f8acf6df680795ce61cc8383e60b5b7d722
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 3%

---


# 第3章 —イベントコンテンツフラグメントのオーサリング

AEMヘッドレスチュートリアルの第3章では、第2 [章で作成したコンテンツフラグメントモデルから、イベントのコンテンツフラグメントを作成し、オーサリングする方法について説明します](./chapter-2.md)。

## イベントコンテンツフラグメントのオーサリング

コンテンツフラグメントモデルを作成し、WKND用のAEM設定をア [!DNL Event] セットフォルダーに( `/content/dam/wknd-mobile` プロパティを使用して)適用すると、 `cq:conf`[!DNL Event] コンテンツフラグメントを作成できます。

アセットの一種であるコンテンツフラグメントは、他のアセットと同様に、AEM Assetsで整理、管理する必要があります。

* 変換が必要な場合（または必要な場合）は、Assetsフォルダー構造のロケールフォルダーを使用します。
* コンテンツフラグメントを論理的に整理し、見つけやすく管理しやすくする

この手順では、 [!DNL Event] assetsフォルダーにの新しいフォルダ `Punkrock Fest``/content/dam/wknd-mobile/en/events` ーを作成します。

1. **[!UICONTROL /]Assets[!UICONTROL /]Files[!UICONTROL /]Files[!DNL WKND Mobile]/create Asset AEMに移動します。[!DNL English]****[!DNL Events]**/Assetフォルダを作成します。
1. ア **[!UICONTROL セット]/[!UICONTROL ファイル]/[!DNL WKND Mobile]/[!DNL English][!DNL Events]****[!DNL Event]****[!DNL Punkrock Fest]**&#x200B;を選択して、新しいコンテンツフラグメントを作成します。タイトルをofのアセットを持つコンテンツフラグメントタイプを作成します。
1. 新しく作成された [!DNL Event] コンテンツフラグメントを作成します。

   * [!DNL Event Title]：**[!DNL Punkrock Fest]**
   * [!DNL Event Description] : **&lt;数行の説明を入力…>**
   * [!DNL Event Date] : **&lt;未来の日付を選択>**
   * [!DNL Event Type] : **音楽**
   * [!DNL Ticket Price] : **10**
   * [!DNL Event Image] : **/content/dam/wknd-mobile/images/tom-rogerson-574325-unsplash.jpg**
   * [!DNL Venue Name] : **ザレプティルハウス**
   * [!DNL Venue City] : **ニューヨーク**

   上部のアクションバーで **[!UICONTROL 「保存]** 」をタップして、変更を保存します。

1. AEM Package Manager [を使用して](http://localhost:4502/crx/packmgr/index.jsp)、AEM Authorに次のパッケージをインストールします。 このパッケージには、多数のイベントコンテンツフラグメントが含まれています。

   [ファイルの取得：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

>[!VIDEO](https://video.tv.adobe.com/v/28338/?quality=12&learn=on)

## コンテンツフラグメントのJCR構造の確認

*この節は情報提供のみを目的としており、コンテンツフラグメントモデルから作成されるコンテンツフラグメントの基盤となるJCR構造を関連付けることを目的としています。*

1. AEM作成者で **[CRXDE Lite](http://localhost:4502/crx/de/index.jsp)** を開きます。
1. CRXDE Liteで、左側の階層メニューで、 [/content/dam/wknd-mobile/en/イベント/punkrock-fest/jcr:content](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content) (JCRの [!DNL Punkrock Fest][!DNL Event] コンテンツフラグメントを表すノード)に移動します。
1. [data](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) ノードを展開します。
「 **プロパティ** 」ペインで、 `cq:model`[!DNL Event] 「コンテンツフラグメントモデル」定義を指すプロパティがあることを確認します。
   * **`cq:model`**=**`/conf/settings/wknd-mobile/dam/cfm/models/event`**
1. ノードの下で `data` マスター [](http://localhost:4502/crx/de/index.jsp#/content/dam/wknd-mobile/en/events/punkrock-fest/jcr:content/data/master) ノードを選択し、プロパティを確認します。 このノードには、コンテンツフラグメントモデルのオーサリング中に収集された [!DNL Event] コンテンツが含まれます。 JCRのプロパティ名はコンテンツフラグメントモデルのプロパティ名に対応し、値は「[!DNL Punkrock Fest][!DNL Event] 」コンテンツフラグメントの作成された値に対応します。

>[!VIDEO](https://video.tv.adobe.com/v/28356/?quality=12&learn=on)

## 次の手順

AEM [Package Managerを使用して、AEM Authorに](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip [コンテンツパッケージをインストールすることをお勧めします ](http://localhost:4502/crx/packmgr/index.jsp)。 このパッケージには、チュートリアルのこの章と前の章で概要を説明している設定と内容が含まれています。

* [第4章 — AEM Content Servicesテンプレートの定義](./chapter-4.md)
