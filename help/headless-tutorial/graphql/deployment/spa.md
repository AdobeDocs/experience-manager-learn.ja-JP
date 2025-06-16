---
title: AEM GraphQL 用の SPA のデプロイ
description: シングルページアプリ（SPA）の AEM ヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
exl-id: 3fe175f7-6213-439a-a02c-af3f82b6e3b7
duration: 136
source-git-commit: 6425188da75f789b0661ec9bfb79624b5704c92b
workflow-type: tm+mt
source-wordcount: '640'
ht-degree: 99%

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

| SPA の接続先 → | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
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

| SPA の接続先 → | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
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

<!-- CARDS 

* ../example-apps/react-app.md

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="React App - AEM Headless Example">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="../example-apps/react-app.md" title="React アプリ - AEM ヘッドレスの例" target="_blank" rel="referrer">
                        <img class="is-bordered-r-small" src="../example-apps/assets/react-app/react-app.png" alt="React アプリ - AEM ヘッドレスの例"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="../example-apps/react-app.md" target="_blank" rel="referrer" title="React アプリ - AEM ヘッドレスの例">React アプリ - AEM ヘッドレスの例 </a>
                    </p>
                    <p class="is-size-6">サンプルアプリケーションは、Adobe Experience Manager（AEM）のヘッドレス機能を調べるうえで役に立ちます。 この React アプリケーションは、永続クエリを使用して AEM GraphQL API でコンテンツに対してクエリを実行する方法を示します。</p>
                </div>
                <a href="../example-apps/react-app.md" target="_blank" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


