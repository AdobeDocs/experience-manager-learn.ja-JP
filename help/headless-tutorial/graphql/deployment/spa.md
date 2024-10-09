---
title: AEM GraphQL 用の SPA のデプロイ
description: シングルページアプリ（SPA）の AEM ヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: f1b13bba9e83ac1d25f2af23ff2673554726eb19
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# AEM ヘッドレス SPA デプロイメント

AEM ヘッドレスシングルページアプリ（SPA）のデプロイメントには、React や Vue などのフレームワークを使用して作成された JavaScript ベースのアプリケーションが含まれます。これらのアプリケーションは、AEM のコンテンツをヘッドレスで使用および操作します。

AEM をヘッドレスで操作する SPA をデプロイするには、SPA をホストし、web ブラウザー経由でアクセスできるようにする必要があります。

## SPA のホスト

SPA は、**HTML、CSS、JavaScript** のネイティブ web リソースのコレクションで構成されます。これらのリソースは、_ビルド_&#x200B;プロセス（`npm run build` など）中に生成され、エンドユーザーが使用できるようにホストにデプロイされます。

組織の要件に応じて、様々な&#x200B;**ホスティング**&#x200B;オプションがあります。

1. **Azure** や **AWS** などの&#x200B;**クラウドプロバイダー**。

2. 企業&#x200B;**データセンター**&#x200B;での&#x200B;**オンプレミス**&#x200B;ホスティング

3. **AWS Amplify**、**Azure App Service**、**Netlify**、**Heroku**、**Vercel** などの&#x200B;**フロントエンドホスティングプラットフォーム**。

## デプロイメント設定

AEM ヘッドレスを操作する SPA をホストする場合の主な考慮事項は、SPA には AEM のドメイン（またはホスト）経由でアクセスするか、様々なドメインでアクセスするかです。その理由は、SPA は web ブラウザーで実行される web アプリケーションであり、web ブラウザーのセキュリティポリシーの対象となるからです。

### 共有ドメイン

SPA と AEM は、両方が同じドメインからエンドユーザーによってアクセスされる場合、ドメインを共有します。次に例を示します。

+ AEM には `https://wknd.site/` 経由でアクセスできます
+ SPA には `https://wknd.site/spa` 経由でアクセスできます

AEM と SPA の両方に同じドメインからアクセスできるので、web ブラウザーは、SPA が CORS を必要とせずに XHR を AEM ヘッドレスエンドポイントに作成し、HTTP Cookie（AEM の `login-token` Cookie など）を共有できるようにします。

SPA と AEM のトラフィックが共有ドメインでどのようにルーティングされるかは、ユーザー次第です。つまり、複数のオリジンを持つ CDN、リバースプロキシを持つ HTTP サーバー、AEM で直接 SPA をホストするなどです。

AEM と同じドメインでホストされている場合に、SPA 実稼動デプロイメントに必要なデプロイメント設定を以下に示します。

| SPAから→への接続 | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| クロスオリジンリソース共有（CORS） | ✘ | ✘ | ✘ |
| AEM ホスト | ✘ | ✘ | ✘ |

### 異なるドメイン

異なるドメインからエンドユーザーがアクセスする場合、SPA と AEM には異なるドメインがあります。次に例を示します。

+ AEM には `https://wknd.site/` 経由でアクセスできます
+ SPA には `https://wknd-app.site/` 経由でアクセスできます

AEM と SPA には異なるドメインからアクセスできるので、web ブラウザーは[クロスオリジンリソース共有（CORS）](./configurations/cors.md)などのセキュリティポリシーを適用し、HTTP Cookie（AEM の `login-token` Cookie など）の共有を防ぎます。

AEM と異なるドメインでホストされている場合に、SPA 実稼動デプロイメントに必要なデプロイメント設定を以下に示します。

| SPAから→への接続 | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [クロスオリジンリソース共有（CORS）](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEM ホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 異なるドメインでの SPA デプロイメントの例

この例では、SPA は Netlify ドメイン（`https://main--sparkly-marzipan-b20bf8.netlify.app/`）にデプロイされ、SPA は AEM パブリッシュドメイン（`https://publish-p65804-e666805.adobeaemcloud.com`）から AEM GraphQL API を使用します。以下のスクリーンショットは、CORS 要件をハイライト表示しています。

1. SPA は Netlify ドメインから提供されますが、異なるドメインで AEM GraphQL API への XHR 呼び出しを行います。このクロスサイトリクエストでは、AEM で [CORS](./configurations/cors.md) を設定して、Netlify ドメインからのリクエストがそのコンテンツにアクセスできるようにする必要があります。

   ![SPA および AEM ホストから提供される SPA リクエスト](assets/spa/cors-requirement.png)

2. AEM GraphQL API への XHR リクエストを検査すると、`Access-Control-Allow-Origin` が存在し、この Netlify ドメインからのリクエストがそのコンテンツにアクセスすることを AEM が許可することを web ブラウザーに示します。

   AEM [CORS](./configurations/cors.md) が見つからなかったか、Netlify ドメインが含まれていなかった場合、web ブラウザーは XHR リクエストに失敗し、CORS エラーを報告します。

   ![CORS 応答ヘッダー AEM GraphQL API](assets/spa/cors-response-headers.png)

## シングルページアプリの例

アドビは、React でコード化された単一ページアプリの例を提供しています。

<div class="columns is-multiline">
<!-- React app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/react-app.md" title="React アプリ" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app-card.png" alt="React アプリ">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/react-app.md" title="React アプリ">React アプリ</a></p>
               <p class="is-size-6">AEM ヘッドレス GraphQL API のコンテンツを使用する、React で記述された単一ページアプリの例です。</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
           </div>
       </div>
   </div>
</div>
<!-- Next.js app -->
<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Next.js app" tabindex="0">
   <div class="card">
       <div class="card-image">
           <figure class="image is-16by9">
               <a href="../example-apps/next-js.md" title="Next.js アプリケーション" tabindex="-1">
                   <img class="is-bordered-r-small" src="../example-apps/assets/next-js/next-js-card.png" alt="Next.js アプリケーション">
               </a>
           </figure>
       </div>
       <div class="card-content is-padded-small">
           <div class="content">
               <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/next-js.md" title="Next.js アプリケーション">Next.js アプリケーション</a></p>
               <p class="is-size-6">AEM ヘッドレス GraphQL API のコンテンツを使用する Next.js で記述された単一ページアプリの例です。</p>
               <a href="../example-apps/next-js.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
           </div>
       </div>
   </div>
</div>
</div>
