---
title: AEM ヘッドレス web コンポーネントデプロイメント
description: Web コンポーネント／純粋な JS ベースの AEM ヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10797
thumbnail: kt-10797.jpg
exl-id: 9d4aab4c-82af-4917-8c1b-3935f19691e6
duration: 31
source-git-commit: 089bcf71f03bdbb6d21337cc23452afb33ce8098
workflow-type: ht
source-wordcount: '164'
ht-degree: 100%

---

# AEM ヘッドレス web コンポーネントデプロイメント

AEM ヘッドレス [web コンポーネント](https://developer.mozilla.org/en-US/docs/Web/Web_Components)／JS デプロイメントは、web ブラウザーで動作し、AEM のコンテンツをヘッドレス方式で消費し操作する純粋な JavaScript アプリです。Web コンポーネント／JS デプロイメントは、堅牢な SPA フレームワークを使用せず、任意の web サイトのコンテキストに埋め込まれて AEM のコンテンツを表示することが想定される点で、[SPA デプロイメント](./spa.md)とは異なります。


## デプロイメント設定

Web コンポーネント／JS デプロイメントでは、次のデプロイメント設定が必要です。

| Web コンポーネント／JS アプリの接続先 → | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [クロスオリジンリソース共有（CORS）](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM ホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## Web コンポーネントの例

アドビでは、web コンポーネントの例を提供しています。

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
                   <p class="is-size-6">AEM ヘッドレス GraphQL API のコンテンツを使用する、純粋な JavaScript で記述された web コンポーネントの例です。</p>
                   <a href="../example-apps/web-component.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
