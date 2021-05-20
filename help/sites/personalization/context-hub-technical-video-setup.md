---
title: AEM Sitesを使用したパーソナライゼーションのためのContextHubの設定
description: ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用してストアにアクセスし、必要に応じてデータを作成、更新および削除できます。したがって、ContextHub はページ上のデータレイヤーに相当します。 このページでは、AEMサイトページにContext Hubを追加する方法について説明します。
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: パーソナライズ機能
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 35%

---


# パーソナライゼーション用のContextHubのセットアップ{#set-up-contexthub}

ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用してストアにアクセスし、必要に応じてデータを作成、更新および削除できます。したがって、ContextHub はページ上のデータレイヤーに相当します。 このページでは、AEMサイトページにContext Hubを追加する方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>このビデオではWKNDリファレンスサイトを使用していますが、AEMリリースには含まれていません。 [最新バージョンは](https://github.com/adobe/aem-guides-wknd/releases)からダウンロードできます。

ContextHub機能を有効にし、ContextHub JavaScriptライブラリにリンクするには、ContextHubをページに追加します。 ContextHub JavaScript APIを使用すると、ContextHubが管理するコンテキストデータにアクセスできます。

## ページコンポーネントへの ContextHub の追加 {#adding-contexthub-to-a-page-component}

ContextHub機能を有効にし、ContextHub JavaScriptライブラリにリンクするには、Webページの`<head>`セクションに`contexthub`コンポーネントを含めます。 ページコンポーネントのHTLコードは、次の例のようになります。

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## サイトの設定とContextHubセグメント{#site-configuration-and-contexthub-segments}

ContextHub には、セグメントの管理や、現在のコンテキストで解決されるセグメントの判断をするセグメント化エンジンが含まれています。いくつかのセグメントが定義されています。JavaScript API を使用して、[解決されたセグメントを判断](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)できます。[[!UICONTROL 設定ブラウザー]](https://docs.adobe.com/content/help/ja-JP/experience-manager-cloud-service/implementing/developing/configurations.html)で、サイトのContextHubセグメントを有効にします。

## セグメントの作成{#create-segments}

ティーザーのルールとして機能するAEMセグメントを作成します。 つまり、ティーザー内のコンテンツがWebページに表示されるタイミングを定義します。 一致するセグメントに基づいて、訪問者のニーズと関心に特化したコンテンツを表示できます。

## クラウド設定、セグメントパス、ContextHubパスのサイトへの割り当て{#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

クラウド設定のパス、セグメント化のパス、ContextHubのパスをサイトのルートノードに割り当てて、オーディエンスに合わせてパーソナライズされたエクスペリエンスを作成できます。 ContextHubを使用すると、コンテキストデータを操作し、解決されたセグメントをテストできます。

![CRXDE Lite](assets/crx-de-properties.png)

ContextHubとセグメント化について詳しくは、以下を参照してください。

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [ページへのContext Hubの追加とストアへのアクセス](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [セグメント化について](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [ContextHub でのセグメント化の設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
