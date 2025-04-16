---
title: AEM Sites のビデオおよびチュートリアル
description: Adobe Experience Manager（AEM）Sites は、優れたエクスペリエンス管理プラットフォームです。このユーザーガイドには、AEM Sites の多くの機能に関するビデオとチュートリアルが含まれています。
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 5bd752ec257763e6d5da99c3da4b44fa9a43fd25
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 24%

---

# AEM Sites のビデオおよびチュートリアル {#overview}

{{edge-delivery-services}}

Adobe Experience Manager（AEM）Sites は、優れたエクスペリエンス管理プラットフォームです。このユーザーガイドには、AEM Sites の多くの機能に関するビデオとチュートリアルが含まれています。

## AEM Sitesで構築する 3 つの方法

AEM Sitesには、エクスペリエンスを作成、オーサリングおよび提供する 3 つの方法があります。 フルページを作成する場合でも、エッジのパフォーマンスに合わせて最適化する場合でも、ヘッドレスアプリを強化する場合でも、AEM Sitesはプロジェクトニーズに合わせた柔軟なオプションを提供します。

1. **従来のサイト** AEM オーサーのページエディターを使用してコンテンツを作成した後、それをアクティブ化して、HTML web ページとしてAEM パブリッシュによってエンドユーザーに配信します。
1. **Edge Delivery Services** web サイトでは、ドキュメントベースのオーサリングまたはAdobe ユニバーサルエディターを活用してコンテンツを作成します。作成したコンテンツはアクティブ化され、Edge Delivery Services as HTML web ページによってエンドユーザーに配信されます。
1. **ヘッドレス/API ファースト** web エクスペリエンスでは、コンテンツフラグメントエディターまたはユニバーサルエディターを使用してコンテンツを作成したあと、それをアクティブ化して、AEM公開で JSON 形式で配信します。

これらのオプションは、マーケティング組織の様々なニーズを満たし、あらゆるチャネルやデバイスにわたって、パーソナライズされた魅力的なエクスペリエンスを高速かつ大規模に提供するように設計されています。

次の図に、様々なパスを示します。

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### AEM Sitesを使用して構築する方法を比較する

次の表は、3 つのパスを大まかに比較したものです。 各パスのコンテンツオーサリングとエクスペリエンス配信のニュアンスに重点を置いています。

|            | 従来のサイト | Edge Delivery Services | ヘッドレス/API ファースト |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **オーサリングツール** | ページエディター | ドキュメントベースのオーサリング、ユニバーサルエディター | コンテンツフラグメント、ユニバーサルエディター |
| **作成したコンテンツストア** | AEM オーサー（JCR） | ドキュメントまたはAEM オーサー（JCR） | AEM オーサー（JCR） |
| **配信** | AEM パブリッシュ（Adobe CDN + Dispatcherを使用） | Edge Delivery Services | AEM パブリッシュ（Adobe CDN + Dispatcherを使用） |
| **配信コンテンツストア** | AEM パブリッシュ（JCR） | Edge Delivery Services | AEM パブリッシュ（JCR） |
| **配信形式** | HTML | HTML | JSON |
| **開発技術** | Java™、JavaScript、CSS | JavaScript、CSS | いずれか（例：Swift、React など） |

## チュートリアル

次のチュートリアルを通じて、AEM Sitesで構築する 3 つのパスについて説明します。

<!-- CARDS

* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional Sites - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
* https://www.aem.live/docs/
  {title = Edge Delivery Services - Guides}
  {description = Explore Edge Delivery Services with comprehensive guides. The Build, Publish, and Launch guides cover everything you need to get started with EDS.}
  {image = ./assets/edge-delivery-services.png}
  {target = _blank}
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview
  {title = Headless/API-First - Tutorials}
  {description = Learn how to build headless applications powered by AEM content. Tutorials cover frameworks like iOS, Android, and React—choose what fits your stack.}
  {image = ./assets/headless.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional Sites - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="従来のサイト - WKND チュートリアル" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="従来のサイト - WKND チュートリアル"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="従来のサイト - WKND チュートリアル"> 従来のサイト - WKND チュートリアル </a>
                    </p>
                    <p class="is-size-6">WKND チュートリアルを使用して、サンプル AEM Sites プロジェクトを作成する方法を説明します。 このガイドでは、プロジェクトの設定、コアコンポーネント、編集可能なテンプレート、クライアントサイドライブラリ、コンポーネント開発について順を追って説明します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Edge Delivery Services - Guides">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://www.aem.live/docs/" title="Edge Delivery Services - ガイド" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/edge-delivery-services.png" alt="Edge Delivery Services - ガイド"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - ガイド">Edge Delivery Services - ガイド </a>
                    </p>
                    <p class="is-size-6">包括的なガイドでEdge Delivery Servicesを参照します。 EDS を使い始めるために必要な情報はすべて、ビルド、パブリッシュ、ローンチのガイドに記載されています。</p>
                </div>
                <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Headless/API-First - Tutorials">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="ヘッドレス/API ファースト – チュートリアル" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="ヘッドレス/API ファースト – チュートリアル"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="ヘッドレス/API ファースト – チュートリアル"> ヘッドレス/API ファースト – チュートリアル </a>
                    </p>
                    <p class="is-size-6">AEM コンテンツを利用したヘッドレスアプリケーションを構築する方法について説明します。 チュートリアルでは、iOS、Android™、React などのフレームワークをカバーし、スタックに合ったものを選択します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## その他のリソース

* [AEM Sites オーサリングドキュメント](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites 開発ドキュメント](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites 管理ドキュメント](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites 導入ドキュメント](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [AEM as a Cloud Service のチュートリアル](/help/cloud-service/overview.md)
* [AEM Assets のチュートリアル](/help/assets/overview.md)
* [AEM Forms のチュートリアル](/help/forms/overview.md)
* [AEM の基盤のチュートリアル](/help/foundation/overview.md)
