---
title: AEM Sitesでのパーソナライゼーション用にContextHubをセットアップ
description: ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub Javascript APIを使用すると、ストアにアクセスし、必要に応じてデータを作成、更新、削除できます。 したがって、ContextHub はページ上のデータレイヤーに相当します。 このページでは、AEMサイトページにコンテキストハブを追加する方法を説明します。
feature: context-hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 1faf22f2e664b775c11e16cb1dfa18b363a7316b
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 25%

---


# パーソナライゼーション用のContextHubの設定 {#set-up-contexthub}

ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub Javascript APIを使用すると、ストアにアクセスし、必要に応じてデータを作成、更新、削除できます。 したがって、ContextHub はページ上のデータレイヤーに相当します。 このページでは、AEMサイトページにコンテキストハブを追加する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/23765/?quality=9&learn=on)

>[!NOTE]
>
>このビデオではWKNDリファレンスサイトを使用していますが、AEMリリースには含まれていません。 最 [新バージョンはこちらからダウンロードできます](https://github.com/adobe/aem-guides-wknd/releases)。

ContextHub追加をページに追加して、ContextHub機能を有効にしたり、ContextHub JavaScriptライブラリにリンクしたりできます。 ContextHub JavaScript APIを使用すると、ContextHubで管理されるコンテキストデータにアクセスできます。

## ページコンポーネントへの ContextHub の追加 {#adding-contexthub-to-a-page-component}

To enable the ContextHub features and to link to the ContextHub JavaScript libraries, include the `contexthub` component in the `<head>` section of your web page. ページコンポーネントのHTLコードは、次の例のようになります。

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
*/-->
```

## サイト設定とContextHubセグメント {#site-configuration-and-contexthub-segments}

ContextHub には、セグメントの管理や、現在のコンテキストで解決されるセグメントの判断をするセグメント化エンジンが含まれています。いくつかのセグメントが定義されています。JavaScript API を使用して、[解決されたセグメントを判断](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments)できます。[ [[!UICONTROL 設定ブラウザ]](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/configurations.html)]で、サイトのContextHubセグメントを有効にします。

## セグメントの作成 {#create-segments}

ティーザーのルールとして機能するAEMセグメントを作成します。 つまり、Webページにテーザー内のコンテンツを表示するタイミングを定義します。 一致するセグメントに基づいて、訪問者のニーズと関心に特化したコンテンツを表示できます。

## サイトへのクラウド設定、セグメントパス、ContextHubパスの割り当て {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

クラウド設定のパス、セグメント化のパス、ContextHubパスをサイトのルートノードに割り当てることで、オーディエンスに合わせてパーソナライズされたエクスペリエンスを作成できます。 ContextHubを使用すると、コンテキストデータを操作し、解決されたセグメントをテストできます。

![CRXDE Lite](assets/crx-de-properties.png)

ContextHubとセグメントについて詳しくは、以下を参照してください。

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [ページへのContext Hubの追加とストアへのアクセス](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [セグメント化について](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [ContextHub でのセグメント化の設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
