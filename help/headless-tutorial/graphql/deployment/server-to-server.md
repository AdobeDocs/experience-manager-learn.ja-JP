---
title: AEM ヘッドレスサーバー間デプロイメント
description: サーバーからサーバーへの AEM ヘッドレスデプロイメントにおける考慮事項について説明します。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
jira: KT-10798
thumbnail: kt-10798.jpg
exl-id: d4ae08d9-dc43-4414-ab75-26853186a301
duration: 48
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 100%

---

# AEM ヘッドレスサーバー間デプロイメント

AEM ヘッドレスサーバー間デプロイメントには、AEM のコンテンツをヘッドレスな方法で消費し、やり取りするサーバー側のアプリケーションまたはプロセスが含まれます。

AEMヘッドレス API への HTTP 接続はブラウザーのコンテキストでは開始されないので、サーバー間デプロイメントには最小限の設定が必要です。

## デプロイメント設定

サーバー間アプリケーションのデプロイメントを実行するには、次のデプロイメント設定をインプレースにする必要があります。

| サーバー間アプリケーションの接続先 → | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|---------------------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| クロスオリジンリソース共有（CORS） | ✘ | ✘ | ✘ |
| [AEM ホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

## 認証要件

AEM GraphQL API への承認されたリクエストは、通常サーバー間アプリのコンテキストで発生します。他のアプリタイプ、例えば、[シングルページアプリ](./spa.md)、[モバイル](./mobile.md)または [web コンポーネント](./web-component.md)では認証情報を保護するのが難しいため、通常は、承認が使用されます。

AEM as a Cloud Service へのリクエストを承認する場合、[サービス資格情報ベースのトークン認証](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/generating-access-tokens-for-server-side-apis.html?lang=ja)を使用します。AEM as a Cloud Serviceへのリクエストの認証の詳細については、[トークンベース認証チュートリアル](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=ja)をご覧ください。このチュートリアルでは、[AEM Assets HTTP API](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets.html?lang=ja) を使用してトークン ベースの認証について説明しますが、同じコンセプトとアプローチは、AEM ヘッドレス GraphQL API とやり取りするアプリにも適用できます。

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
                   <p class="is-size-6">AEM ヘッドレス GraphQL API のコンテンツを使用する Node.js で記述された、サーバー間アプリの例です。</p>
                   <a href="../example-apps/server-to-server-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                       <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
                   </a>
               </div>
           </div>
       </div>
    </div>
</div>
