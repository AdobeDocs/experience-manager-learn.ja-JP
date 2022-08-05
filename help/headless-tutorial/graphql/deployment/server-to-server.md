---
title: AEMヘッドレスサーバー間デプロイメント
description: サーバー間AEMヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10798
thumbnail: kt-10798.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# AEMヘッドレスサーバー間デプロイメント

AEMヘッドレスサーバー間デプロイメントには、AEMのコンテンツをヘッドレスな方法で消費し、やり取りするサーバー側のアプリケーションまたはプロセスが含まれます。

AEMヘッドレス API への HTTP 接続はブラウザーのコンテキストでは開始されないので、サーバー間デプロイメントには最小限の設定が必要です。

## デプロイメント設定

サーバー間アプリケーションのデプロイメントを実行するには、次のデプロイメント設定をインプレースにする必要があります。

| サーバー間アプリケーションの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| クロスオリジンリソース共有 (CORS) | ✘ | ✘ | ✘ |
| [AEMホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 認証要件

AEM GraphQL API に対する承認済みのリクエストは、通常、サーバー間アプリのコンテキストで発生します。これは、他のアプリタイプ ( [シングルページアプリ](./spa.md), [モバイル](./mobile.md)または [Web コンポーネント](./web-component.md)では通常、認証を使用します。資格情報を保護するのは困難です。

AEM as a Cloud Serviceへのリクエストを認証する場合は、 [サービス資格情報ベースのトークン認証](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html). AEM as a Cloud Serviceへのリクエストの認証について詳しくは、 [トークンベースの認証に関するチュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html). このチュートリアルでは、 [AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html) ただし、AEMヘッドレス GraphQL API とやり取りするアプリにも、同じ概念とアプローチが適用されます。

## サーバー間アプリの例

Adobeは、Node.js でコード化されたサーバー間アプリの例を提供しています。

<div class="columns is-multiline">
    <!-- Server-to-server app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Server-to-server app" tabindex="0">
       <div class="card">
           <div class="card-image">
               <figure class="image is-16by9">
                   <a href="../example-apps/server-to-server-app.md" title="サーバー間アプリ" tabindex="-1">
                       <img class="is-bordered-r-small" src="../example-apps/assets/server-to-server-app/server-to-server-card.png" alt="サーバー間アプリ">
                   </a>
               </figure>
           </div>
           <div class="card-content is-padded-small">
               <div class="content">
                   <p class="headline is-size-6 has-text-weight-bold"><a href="../example-apps/server-to-server-app.md" title="サーバー間アプリ">サーバー間アプリ</a></p>
                   <p class="is-size-6">AEMヘッドレス GraphQL API のコンテンツを使用する Node.js で記述された、サーバー間アプリの例です。</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>