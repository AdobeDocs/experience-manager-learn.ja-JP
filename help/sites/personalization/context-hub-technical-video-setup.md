---
title: AEM Sites を使用したパーソナライズ機能のための ContextHub のセットアップ
description: ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用してストアにアクセスし、必要に応じてデータを作成、更新および削除できます。したがって、ContextHub はページ上のデータレイヤーを表します。 このページでは、AEMサイトページに Context Hub を追加する方法について説明します。
feature: Context Hub
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 366
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 93%

---

# パーソナライズ機能用の ContextHub のセットアップ {#set-up-contexthub}

ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用してストアにアクセスし、必要に応じてデータを作成、更新および削除できます。したがって、ContextHub はページ上のデータレイヤーに相当します。このページでは、AEM サイトページに Context Hub を追加する方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>このビデオでは WKND リファレンスサイトを使用しており、AEM リリースには含まれていません。 [こちらから最新バージョン](https://github.com/adobe/aem-guides-wknd/releases)をダウンロードできます。

ContextHub 機能を有効にし、ContextHub JavaScript ライブラリにリンクするには、ContextHub をページに追加します。ContextHub JavaScript API を使用すると、ContextHub が管理するコンテキストデータにアクセスできます。

## ページコンポーネントへの ContextHub の追加 {#adding-contexthub-to-a-page-component}

ContextHub 機能を有効にし、ContextHub JavaScript ライブラリにリンクするには、web ページの `<head>` セクションに `contexthub` コンポーネントを含めます。ページコンポーネントの HTL コードは、次の例のようになります。

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## サイト設定と ContextHub セグメント {#site-configuration-and-contexthub-segments}

ContextHub には、セグメントの管理や、現在のコンテキストで解決されるセグメントを判断するセグメント化エンジンが含まれています。いくつかのセグメントが定義されています。JavaScript API を使用して、[解決されたセグメントを判断](https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/ch-adding.html?lang=ja)できます。[[!UICONTROL 設定ブラウザー]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=ja) の下で、サイトの ContextHub セグメントを有効にします。

## セグメントの作成 {#create-segments}

ティーザーのルールとして機能する AEM セグメントを作成します。 つまり、ティーザー内のコンテンツが web ページにいつ表示されるかを定義します。 一致するセグメントに基づいて、訪問者のニーズと関心に特化したコンテンツを表示できます。

## サイトへのクラウド設定、セグメントパス、ContextHub パスの割り当て {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

クラウド設定のパス、セグメント化のパス、ContextHub のパスをサイトのルートノードに割り当てて、オーディエンスに合わせてパーソナライズされたエクスペリエンスを作成できます。ContextHub を使用すると、コンテキストデータを操作し、解決したセグメントをテストすることができます。

![CRXDE Lite](assets/crx-de-properties.png)

ContextHub とセグメント化について詳しくは、以下を参照してください。

* [ContextHub](https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/contexthub.html?lang=ja)
* [ページへの Context Hub の追加とストアへのアクセス](https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/ch-adding.html?lang=ja)
* [セグメント化について](https://experienceleague.adobe.com/docs/experience-manager-65/classic-ui/personalization/classic-personalization-campaigns-segmentation.html?lang=ja)
* [ContextHub でのセグメント化の設定](https://experienceleague.adobe.com/docs/experience-manager-65/administering/personalization/segmentation.html?lang=ja)
