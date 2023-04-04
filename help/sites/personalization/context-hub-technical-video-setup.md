---
title: AEM Sitesを使用したパーソナライゼーションのための ContextHub のセットアップ
description: ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用してストアにアクセスし、必要に応じてデータを作成、更新および削除できます。したがって、ContextHub はページ上のデータレイヤーに相当します。 このページでは、AEMサイトページに Context Hub を追加する方法について説明します。
feature: Context Hub
topics: personalization
audience: developer, architect
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: Personalization
role: Developer
level: Intermediate
exl-id: 89308dd3-a7e5-4fec-bffb-5f0974125c0a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '424'
ht-degree: 23%

---

# パーソナライゼーション用の ContextHub のセットアップ {#set-up-contexthub}

ContextHub は、コンテキストデータを保存、操作および表示するためのフレームワークです。ContextHub JavaScript API を使用してストアにアクセスし、必要に応じてデータを作成、更新および削除できます。したがって、ContextHub はページ上のデータレイヤーに相当します。 このページでは、AEMサイトページに Context Hub を追加する方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/23765?quality=12&learn=on)

>[!NOTE]
>
>このビデオでは WKND リファレンスサイトを使用しており、AEMリリースには含まれていません。 次をダウンロード： [こちらの最新バージョン](https://github.com/adobe/aem-guides-wknd/releases).

ContextHub 機能を有効にし、ContextHub JavaScript ライブラリにリンクするには、ContextHub をページに追加します。 ContextHub JavaScript API を使用すると、ContextHub が管理するコンテキストデータにアクセスできます。

## ページコンポーネントへの ContextHub の追加 {#adding-contexthub-to-a-page-component}

ContextHub 機能を有効にし、ContextHub JavaScript ライブラリにリンクするには、 `contexthub` コンポーネント `<head>` 」セクションに貼り付けます。 ページコンポーネントの HTL コードは、次の例のようになります。

```java
<!--/* Include Context Hub */-->
<sly data-sly-resource="${'contexthub' @ resourceType='granite/contexthub/components/contexthub'}"/>
```

## サイト設定と ContextHub セグメント {#site-configuration-and-contexthub-segments}

ContextHub には、セグメントを管理し、現在のコンテキストに対して解決されるセグメントを決定するセグメント化エンジンが含まれています。 複数のセグメントが定義されています。 JavaScript API を使用すると、 [解決されたセグメントを決定](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html#DeterminingResolvedContextHubSegments). 配下のサイトに対して ContextHub セグメントを有効にする [[!UICONTROL 設定ブラウザー]](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/configurations.html?lang=ja).

## セグメントを作成 {#create-segments}

ティーザーのルールとして機能するAEMセグメントを作成します。 つまり、ティーザー内のコンテンツが Web ページにいつ表示されるかを定義します。 一致するセグメントに応じて、訪問者のニーズと興味に特別にターゲットしたコンテンツを表示できます。

## サイトへのクラウド設定、セグメントパス、ContextHub パスの割り当て {#assigning-cloud-configuration-segment-path-and-contexthub-path-to-your-site}

クラウド設定のパス、セグメント化のパス、ContextHub のパスをサイトのルートノードに割り当てて、オーディエンスに合わせてパーソナライズされたエクスペリエンスを作成できます。 ContextHub を使用すると、コンテキストデータを操作し、解決したセグメントをテストできます。

![CRXDE Lite](assets/crx-de-properties.png)

ContextHub とセグメント化について詳しくは、以下を参照してください。

* [ContextHub](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html)
* [ページへの Context Hub の追加とストアへのアクセス](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/ch-adding.html)
* [セグメント化について](https://helpx.adobe.com/experience-manager/6-5/sites/classic-ui-authoring/using/classic-personalization-campaigns-segmentation.html)
* [ContextHub でのセグメント化の設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/segmentation.html)
