---
title: AEM Sites を使用したパーソナライズ機能のための ContextHub のセットアップ
description: ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用すると、ストアにアクセスして、データを必要に応じて作成、更新および削除できます。したがって、ContextHub はページのデータレイヤーに相当します。このページでは、AEM サイトページに Context Hub を追加する方法について説明します。
feature: Context Hub
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
duration: 375
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '383'
ht-degree: 100%

---

# パーソナライズ機能用の ContextHub のセットアップ {#set-up-contexthub}

ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用すると、ストアにアクセスして、データを必要に応じて作成、更新および削除できます。したがって、ContextHub はページ上のデータレイヤーに相当します。このページでは、AEM サイトページに Context Hub を追加する方法について説明します。

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

ContextHub には、セグメントの管理や、現在のコンテキストで解決されるセグメントを判断するセグメント化エンジンが含まれています。いくつかのセグメントが定義されています。JavaScript API を使用して、[解決されたセグメントを判断](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)できます。[[!UICONTROL 設定ブラウザー]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=ja) の下で、サイトの ContextHub セグメントを有効にします。

## セグメントの作成 {#create-segments}

ティーザーのルールとして機能する AEM セグメントを作成します。 つまり、ティーザー内のコンテンツが web ページにいつ表示されるかを定義します。 一致するセグメントに基づいて、訪問者のニーズと関心に特化したコンテンツを表示できます。

## サイトへのクラウド設定、セグメントパス、ContextHub パスの割り当て {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

クラウド設定のパス、セグメント化のパス、ContextHub のパスをサイトのルートノードに割り当てて、オーディエンスに合わせてパーソナライズされたエクスペリエンスを作成できます。ContextHub を使用すると、コンテキストデータを操作し、解決したセグメントをテストすることができます。

![CRXDE Lite](assets/crx-de-properties.png)

ContextHub とセグメント化について詳しくは、以下を参照してください。

* [ContextHub](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/contexthub.html)
* [ページへの Context Hub の追加とストアへのアクセス](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [セグメント化について](https://helpx.adobe.com/jp/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [ContextHub でのセグメント化の設定](https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/segmentation.html)
