---
title: AEM Sites のビデオおよびチュートリアル
description: Adobe Experience Manager Sites の特長と機能に関するビデオとチュートリアルをご覧ください。AEM Sites は、優れたエクスペリエンス管理プラットフォームです。
solution: Experience Manager, Experience Manager Sites
sub-product: Experience Manager Sites
topic: Content Management
doc-type: Catalog
exl-id: cde4ce7f-0afe-4632-8c1c-354586f296d5
source-git-commit: 36917be459162e5399620c976bfe953cc5553c82
workflow-type: ht
source-wordcount: '637'
ht-degree: 100%

---

# AEM Sites のビデオおよびチュートリアル {#overview}

{{edge-delivery-services}}

Adobe Experience Manager（AEM）Sites は、優れたエクスペリエンス管理プラットフォームです。このユーザーガイドには、AEM Sites の多くの機能に関するビデオとチュートリアルが含まれています。

## AEM Sites で作成する 3 つの方法

AEM Sites では、エクスペリエンスの作成、オーサリング、配信を行うための 3 つの方法を用意しています。完全なページの作成、エッジパフォーマンスの最適化、ヘッドレスアプリの強化など、AEM Sites では、プロジェクトのニーズに合わせて柔軟なオプションを提供します。

1. **Edge Delivery Services** web サイトでは、ドキュメントベースのオーサリングまたは Adobe ユニバーサルエディターを活用してコンテンツを作成し、アクティブ化して、Edge Delivery Services で HTML web ページとしてエンドユーザーに配信します。このオプションは、主に高いパフォーマンス、スケーラビリティ、速度を必要とする&#x200B;_新規および既存のプロジェクト_&#x200B;向けです。
1. **ヘッドレス／API ファースト** web エクスペリエンスでは、コンテンツフラグメントエディターまたはユニバーサルエディターを使用してコンテンツを作成し、アクティブ化して、AEM パブリッシュで JSON として配信します。このオプションは主に、モバイルアプリ、シングルページアプリケーション（SPA）またはその他のヘッドレスアプリケーションにコンテンツをヘッドレス配信する必要がある&#x200B;_新規および既存のプロジェクト_&#x200B;向けです。
1. **従来の AEM** は、AEM Sites を使って Web エクスペリエンスを構築する最新のアプローチではありません。従来の AEM ユーザーは、AEM オーサーのページエディターを使用してコンテンツを作成し、アクティブ化して、AEM パブリッシュで HTML web ページとしてエンドユーザーに配信します。従来のAEMは、_既存のプロジェクト_&#x200B;に使用することをお勧めします。

これらのオプションは、マーケティング組織の様々なニーズを満たし、あらゆるチャネルやデバイスをまたいで高速かつ大規模にパーソナライズされた魅力的なエクスペリエンスを提供するように設計されています。

>[!IMPORTANT]
>
> **Edge Delivery Services** は、AEM Sitesを使用して構築する最新の方法です。Adobe の Edge Network の機能を活用して、パフォーマンスの高い web サイトを大規模に配信するように設計されています。

次の図に、様々なパスを示します。

![AEM-Sites-Content-Authoring-and-Experience-Delivery-Paths.png](./assets/aem-sites-authoring-and-experience-delivery-paths.png){width="700" zoomable="yes"}

### AEM Sites を使用した作成方法の比較

次の表では、3 つのパスを大まかに比較しています。各パスのコンテンツのオーサリングとエクスペリエンス配信のニュアンスに焦点を当てています。

|            | Edge Delivery Services | ヘッドレス／API ファースト | 従来のAEM |
|---------------------|------------------------------|---------------------------------|---------------------------------------------|
| **次に最適** | 高いトラフィック、パフォーマンス、スケーラビリティが求められる web サイト | モバイルアプリ、SPA、その他のヘッドレスアプリケーション | 既存のプロジェクト （最新のアプローチではない） |
| **オーサリングツール** | ドキュメントベースのオーサリング、ユニバーサルエディター | コンテンツフラグメント、ユニバーサルエディター | ページエディター |
| **作成済みコンテンツストア** | ドキュメントまたは AEM オーサー（JCR） | AEM オーサー（JCR） | AEM オーサー（JCR） |
| **配信** | Edge Delivery Services | AEM パブリッシュ（Adobe CDN + Dispatcher を使用） | AEM パブリッシュ（Adobe CDN + Dispatcher を使用） |
| **配信コンテンツストア** | Edge Delivery Services | AEM パブリッシュ（JCR） | AEM パブリッシュ（JCR） |
| **配信形式** | HTML | JSON | HTML |
| **開発テクノロジー** | JavaScript、CSS | 任意（例：Swift、React など） | Java™、JavaScript、CSS |
| **実装段階** | 新規および既存のプロジェクト | 新規および既存のプロジェクト | 既存のプロジェクトのみ |

## チュートリアル

次のチュートリアルを通じて、AEM Sites で作成する 3 つのパスのそれぞれについて説明します。

<!-- CARDS

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
* https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview
  {title = Traditional AEM - WKND Tutorial}
  {description = Learn how to build a sample AEM Sites project using the WKND tutorial. This guide walks you through project setup, Core Components, Editable Templates, client-side libraries, and component development.}
  {image = ./assets/aem-wknd-spa-editor-tutorial.png}
  {target = _self}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
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
                        <a href="https://www.aem.live/docs/" target="_blank" rel="referrer" title="Edge Delivery Services - ガイド">Edge Delivery Services - ガイド</a>
                    </p>
                    <p class="is-size-6">包括的なガイドを使用して、Edge Delivery Services について説明します。ビルド、パブリッシュ、ローンチガイドでは、EDS の使用を開始するために必要なすべての内容をカバーしています。</p>
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
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/overview" title="ヘッドレス／API ファースト - チュートリアル" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/headless.png" alt="ヘッドレス／API ファースト - チュートリアル"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" title="ヘッドレス／API ファースト - チュートリアル">ヘッドレス／API ファースト - チュートリアル</a>
                    </p>
                    <p class="is-size-6">AEM コンテンツを活用したヘッドレスアプリケーションの作成方法について説明します。チュートリアルでは、iOS、Android、React などのフレームワークをカバーしています。お使いのスタックに適したものを選択してください。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Traditional AEM - WKND Tutorial">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" title="従来のAEM - WKND チュートリアル" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/aem-wknd-spa-editor-tutorial.png" alt="従来のAEM - WKND チュートリアル"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" title="従来のAEM - WKND チュートリアル">従来の AEM - WKND チュートリアル</a>
                    </p>
                    <p class="is-size-6">WKND チュートリアルを使用して、サンプル AEM Sites プロジェクトを作成する方法について説明します。このガイドでは、プロジェクトの設定、コアコンポーネント、編集可能なテンプレート、クライアントサイドライブラリ、コンポーネント開発について説明します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## その他のリソース

* [AEM Sites オーサリングドキュメント](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/authoring/essentials/first-steps)
* [AEM Sites 開発ドキュメント](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/developing/introduction/getting-started)
* [AEM Sites 管理ドキュメント](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/sites/administering/home)
* [AEM Sites 導入ドキュメント](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/deploying/introduction/platform)
* [AEM as a Cloud Service のチュートリアル](/help/cloud-service/overview.md)
* [AEM Assets のチュートリアル](/help/assets/overview.md)
* [AEM Forms のチュートリアル](/help/forms/overview.md)
* [AEM の基盤のチュートリアル](/help/foundation/overview.md)
