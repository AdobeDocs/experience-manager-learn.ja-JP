---
title: AEMヘッドレス Web コンポーネントのデプロイメント
description: Web コンポーネント/純粋な JS ベースのAEMヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 68
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 21%

---

# AEMヘッドレス Web コンポーネントのデプロイメント

AEMヘッドレス [Web コンポーネント](https://developer.mozilla.org/en-US/docs/Web/Web_Components)/JS デプロイメントは、Web ブラウザーで実行され、ヘッドレスにAEMでコンテンツを消費し、操作する純粋な JavaScript アプリです。 Web コンポーネント/JS のデプロイメントは、 [SPAデプロイメント](./spa.md) 堅牢なSPAフレームワークを使用せず、AEMからコンテンツを表示するために任意の web サイトのコンテキストに埋め込まれ、配信されることが期待されます。


## デプロイメント設定

Web コンポーネント/JS デプロイメントを実行するには、次のデプロイメント設定をインプレースにする必要があります。

| Web コンポーネント/JS アプリが | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [クロスオリジンリソース共有（CORS）](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM ホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Web コンポーネントの例

Adobeは、Web コンポーネントの例を提供しています。

<div class="columns is-multiline">
    <!-- Web Component -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/web-component.md" title="Web コンポーネント" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/web-component/web-component-card.png" alt="Web コンポーネント">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/web-component.md" title="Web コンポーネント">Web コンポーネント</a></p>
                   <p class="is-size-6">純粋な JavaScript で記述された、AEMヘッドレスGraphQL API のコンテンツを使用する Web コンポーネントの例です。</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
