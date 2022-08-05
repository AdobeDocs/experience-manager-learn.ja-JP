---
title: SPA for AEM GraphQL のデプロイ
description: シングルページアプリ (SPA)AEMヘッドレスデプロイメントのデプロイメントに関する考慮事項について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 1%

---


# AEMヘッドレスSPAデプロイメント


AEMヘッドレスシングルページアプリ (SPA) のデプロイメントには、React、Vue、Angularなどのフレームワークを使用して構築された JavaScript ベースのアプリケーションが含まれ、AEMのコンテンツをヘッドレスで消費し、操作します。

ヘッドレスな方法でAEMを操作するSPAをデプロイするには、SPAのホストと、Web ブラウザーを使用してアクセスできるようにする必要があります。

## SPAをホスト

SPAは、次のネイティブ Web リソースの集まりで構成されます。 **HTML、CSS および JavaScript**. これらのリソースは、 _ビルド_ プロセス ( 例： `npm run build`) をクリックし、エンドユーザーが使用するためにホストにデプロイします。

種々ある **ホスト** オプションは、組織の要件に応じて以下に示します。

1. **クラウドプロバイダー** 例： **Azure** または **AWS**.

2. **オンプレミス** 企業でのホスティング **データセンター**

3. **フロントエンドホスティングプラットフォーム** 例： **AWS Amplify**, **Azure App Service**, **Netlify**, **辺六**, **ベルセル**&#x200B;など

## デプロイメント設定

AEMヘッドレスとやり取りするSPAをホストする際の主な考慮事項は、SPAがAEMドメイン（またはホスト）経由でアクセスされるか、別のドメインでアクセスされるかです。  理由は、SPAが Web ブラウザーで実行される Web アプリケーションなので、Web ブラウザーのセキュリティポリシーに従うからです。

### 共有ドメイン

SPAとAEMは、両方のドメインが同じドメインのエンドユーザーによってアクセスされる場合、ドメインを共有します。 次に例を示します。

+ AEMには次の経由でアクセスします。 `https://wknd.site/`
+ SPAへのアクセスには `https://wknd.site/spa`

AEMとSPAの両方に同じドメインからアクセスされるので、Web ブラウザーでは、SPAは CORS を必要とせずに XHR をAEMヘッドレスエンドポイントに送信し、HTTP Cookie(AEMなど ) の共有を許可します `login-token` cookie) です。

SPAとAEMのトラフィックが共有ドメイン上でどのようにルーティングされるかは、ユーザー次第です。複数のオリジンを持つ CDN、リバースプロキシを持つ HTTP サーバー、SPAをAEMで直接ホストするなど。

AEMと同じドメインでホストされる場合、SPA実稼動デプロイメントに必要なデプロイメント設定を次に示します。

| SPAの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| クロスオリジンリソース共有 (CORS) | ✘ | ✘ | ✘ |
| AEMホスト | ✘ | ✘ | ✘ |

### 異なるドメイン

SPAとAEMは、異なるドメインのエンドユーザーからアクセスされる場合、異なるドメインを持ちます。 次に例を示します。

+ AEMには次の経由でアクセスします。 `https://wknd.site/`
+ SPAへのアクセスには `https://wknd-app.site/`

AEMとSPAは異なるドメインからアクセスされるので、Web ブラウザーは、次のようなセキュリティポリシーを適用します。 [クロスオリジンリソース共有 (CORS)](./configurations/cors.md)HTTP Cookie(AEMなど ) の共有を禁止する `login-token` cookie) です。

AEMとは異なるドメインでホストされる場合に、SPA実稼動デプロイメントに必要なデプロイメント設定を次に示します。

| SPAの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|---------------------------------------------------:|:----------:|:-----------:|:-----------:|
| [Dispatcher フィルター](./configurations/dispatcher-filters.md) | ✘ | ✔ | ✔ |
| [クロスオリジンリソース共有 (CORS)](./configurations/cors.md) | ✔ | ✔ | ✔ |
| [AEMホスト](./configurations/aem-hosts.md) | ✔ | ✔ | ✔ |

#### 異なるドメインでのSPAデプロイメントの例

この例では、SPAは Netlify ドメイン (`https://main--sparkly-marzipan-b20bf8.netlify.app/`) を使用しSPAは、AEM パブリッシュドメインからAEM GraphQL API を使用します (`https://publish-p65804-e666805.adobeaemcloud.com`) をクリックします。 以下のスクリーンショットは、CORS の要件を示しています。

1. SPAは Netlify ドメインから提供されますが、別のドメインでAEM GraphQL API への XHR 呼び出しをおこないます。 このクロスサイトリクエストには [CORS](./configurations/cors.md) をAEMに設定して、Netlify ドメインからのコンテンツへのアクセス要求を許可します。

   ![SPAおよびAEMホストからのSPAリクエスト ](assets/spa/cors-requirement.png)

2. AEM GraphQL API への XHR リクエストの調査、 `Access-Control-Allow-Origin` が存在し、この Netlify ドメインからのコンテンツへのアクセス要求をAEMが許可していることを Web ブラウザーに示します。

   AEM [CORS](./configurations/cors.md) が見つからなかったか、Netlify ドメインが含まれていなかった場合、Web ブラウザーは XHR リクエストに失敗し、CORS エラーを報告します。

   ![CORS 応答ヘッダーAEM GraphQL API](assets/spa/cors-response-headers.png)

## 単一ページアプリの例

Adobeは、React でコード化された単一ページアプリの例を提供しています。

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
               <p class="is-size-6">AEM Headless GraphQL API のコンテンツを使用する、React で記述された単一ページアプリの例です。</p>
               <a href="../example-apps/react-app.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                   <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
               </a>
           </div>
       </div>
   </div>
</div>
</div>
