---
title: AEM API の概要
description: Adobe Experience Manager（AEM）の様々なタイプの API について学習し、統合用に選択する API を理解します。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17425
thumbnail: KT-17425.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 7cd9efb62d1afdcc089e1e6260d6cf2fc5495afe
workflow-type: ht
source-wordcount: '999'
ht-degree: 100%

---

# AEM API の概要{#aem-apis-overview}

Adobe Experience Manager（AEM）の様々なタイプの API について学習し、統合用に選択する API を理解します。

AEM でコンテンツ、アセット、フォームを作成、読み取り、更新、削除するために、開発者は様々な API を使用できます。これらの API により、開発者は AEM とやり取りするカスタムアプリケーションを作成できます。

AEM の様々なタイプの API を探索し、統合用に選択する API を理解しましょう。

## AEM API のタイプ{#types-of-aem-apis}

AEM は、オーサーサービスとパブリッシュサービスのタイプとやり取りするための次の API を提供します。

| AEM API タイプ | 説明 | 入手方法 | ユースケース | API の例 |
| --- | --- | --- | --- | --- |
| OpenAPI ベースの AEM API | アセット、Sites およびフォーム用の標準化された機械読み取り可能な API。 | **AEM as a Cloud Service のみ** | API ファースト開発、最新のアプリケーション | [Assets Author API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)、[フォルダー API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)、[AEM Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/)、[フォームドキュメントサービス API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/)、その他 |
| RESTful API | AEM リソースとやり取りするための従来の REST エンドポイント。 | AEM 6.X、AEM as a Cloud Service | CRUD 操作、最新のアプリケーション | [アセット HTTP API](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)、[ワークフロー REST API](https://experienceleague.adobe.com/ja/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api)、[コンテンツサービス用の JSON エクスポーター](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter)など |
| GraphQL API | 柔軟なクエリを使用して構造化コンテンツを効率的に取得するために最適化されています。 | AEM 6.X、AEM as a Cloud Service | ヘッドレス CMS、SPA、モバイルアプリ | [GraphQL API](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| 従来の（非 RESTful）API | JCR、Sling Model、Query Builder などの古い API。 | AEM 6.X、AEM as a Cloud Service | レガシー統合、下位互換性 | [Query Builder API](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) など |

詳しくは、[Adobe Experience Manager as a Cloud Service API](https://developer.adobe.com/experience-cloud/experience-manager-apis/) ページを参照してください。

## 選択する API{#which-api-to-choose}

統合用の API を選択する際には、次の要因を考慮します。

- **ユースケース**：AEM API がユースケースをサポートしているかどうかを判断します。可能な限り、AEM とやり取りするための標準化された最新のアプローチを提供する _OpenAPI ベースの AEM API を使用_&#x200B;します。OpenAPI ベースの API を利用できない場合は、RESTful API またはGraphQL API を使用し、最後の手段として従来の API を使用することを検討します。

- **互換性**：選択した API が AEM のバージョンと互換性があることを確認します。例えば、_OpenAPI ベースの AEM API は AEM as a Cloud Service 専用_&#x200B;で、AEM 6.X では使用できません。

- **AEM サービスのタイプ：オーサーとパブリッシュ**：アクセスモデルが異なるので、API の選択もオーサーサービスとパブリッシュサービスのどちらで実行されるかによって異なります。AEM オーサーサービスは、コンテンツの作成に使用され、常に認証が必要です。AEM パブリッシュサービスはコンテンツ配信に使用され、ユースケースによっては認証が不要な場合があります。

- **認証**：使用する認証方法を API がサポートしていることを確認します。例：
   - **OpenAPI ベースの AEM API**：クライアント資格情報（サーバー間）、認証コード（web アプリ）、コード交換のプルーフキー（単一ページアプリ）付与タイプなど、OAuth 2.0 認証をサポートします。その他の AEM API は、OAuth 2.0 認証をサポートしていません。
   - **RESTful API**：JSON web トークン（JWT）認証をサポートし、トークンベースの認証としても認識されています。

## JSON web トークン（JWT）と OAuth 2.0 の違い{#difference-between-jwt-and-oauth}

AEM API で使用される 2 つの一般的な認証メカニズムである、JSON web トークン（JWT）と OAuth 2.0 を比較します。

| 機能 | JSON web トークン（JWT） | OAuth 2.0 |
| --- | --- | --- |
| 使用場所 | RESTful API | OpenAPI ベースの AEM API（RESTful API やその他の API ではサポートされていません） |
| 目的 | サービス認証 | ユーザーまたはサービスの認証 |
| ユーザーインタラクション | ユーザーインタラクションは不要 | 認証コードおよび単一ページアプリ付与タイプに必要なユーザーインタラクション |
| 最適な用途 | サーバー間 API 呼び出し | アプリおよびユーザーのための安全で許可されたアクセス |
| 必要な情報 | JWT に署名するための秘密鍵 | OAuth 2.0 用のクライアント ID とクライアント秘密鍵 |
| トークンの有効期限 | 短時間のみ有効（多くの場合、更新が必要） | アクセストークンは短時間のみ有効です。更新トークンは長期間有効で、新しいアクセストークンの取得に使用されます |
| 資格情報管理 | [AEM Developer Console](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/debugging/debugging-aem-as-a-cloud-service/developer-console) | [Adobe Developer Console](https://developer.adobe.com/developer-console/) |

## OpenAPI ベースの AEM API

OpenAPI ベースの AEM API と、Adobe API へのアクセスに関する重要な概念について詳しくは、[OpenAPI ベースの AEM API](./openapis/overview.md) ガイドを参照してください。

### ユースケース

<!-- CARDS
{target = _self}

* ./openapis/use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./openapis/assets/s2s/OAuth-S2S.png}
* ./openapis/use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./openapis/assets/web-app/OAuth-WebApp.png} 
* ./openapis/use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using OAuth Single Page App}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
  {image = ./openapis/assets/spa/OAuth-SPA.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" title="サーバー間認証を使用した API の呼び出し" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/s2s/OAuth-S2S.png" alt="サーバー間認証を使用した API の呼び出し"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="サーバー間認証を使用した API の呼び出し">サーバー間認証を使用した API の呼び出し</a>
                    </p>
                    <p class="is-size-6">OAuth サーバー間認証を使用して、カスタム NodeJS アプリケーションから OpenAPI ベースの AEM API を呼び出す方法を説明します。</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" title="Web アプリ認証を使用した API の呼び出し" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/web-app/OAuth-WebApp.png" alt="Web アプリ認証を使用した API の呼び出し"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Web アプリ認証を使用した API の呼び出し">Web アプリ認証を使用した API の呼び出し</a>
                    </p>
                    <p class="is-size-6">OAuth web アプリ認証を使用して、カスタム web アプリケーションから OpenAPI ベースの AEM API を呼び出す方法を説明します。</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using OAuth Single Page App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" title="OAuth 単一ページアプリを使用した API の呼び出し" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./openapis/assets/spa/OAuth-SPA.png" alt="OAuth 単一ページアプリを使用した API の呼び出し"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="OAuth 単一ページアプリを使用した API の呼び出し">OAuth 単一ページアプリを使用した API の呼び出し</a>
                    </p>
                    <p class="is-size-6">OAuth 2.0 PKCE フローを使用して、カスタム単一ページアプリ（SPA）から OpenAPI ベースの AEM API を呼び出す方法を説明します。</p>
                </div>
                <a href="./openapis/use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->



## GraphQL API - 例

GraphQL API とその使用方法について詳しくは、[AEM ヘッドレス - GraphQL の概要](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview)を参照してください

### ユースケース

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app
  {title = Single Page Application (SPA)}
  {description = Learn how to build a Single Page Application (SPA) that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/react-app-card.png}
* https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps
  {title = Mobile App}
  {description = Learn how to build a mobile app that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/ios-app-card.png}
* https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component
  {title = Web Component}
  {description = Learn how to build a web component that fetches content from AEM using GraphQL APIs.}
  {image = ./assets/web-component-card.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Single Page Application (SPA)">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" title="単一ページアプリケーション（SPA）" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/react-app-card.png" alt="単一ページアプリケーション（SPA）"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" title="単一ページアプリケーション（SPA）">単一ページアプリケーション（SPA）</a>
                    </p>
                    <p class="is-size-6">GraphQL API を使用して AEM からコンテンツを取得する単一ページアプリケーション（SPA）を作成する方法を説明します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/spa#example-single-page-app" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Mobile App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" title="モバイルアプリ" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/ios-app-card.png" alt="モバイルアプリ"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" title="モバイルアプリ">モバイルアプリ</a>
                    </p>
                    <p class="is-size-6">GraphQL API を使用して AEM からコンテンツを取得するモバイルアプリを作成する方法を説明します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/mobile#example-mobile-apps" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Web Component">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" title="Web コンポーネント" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-component-card.png" alt="Web コンポーネント"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" title="Web コンポーネント">Web コンポーネント</a>
                    </p>
                    <p class="is-size-6">GraphQL API を使用して AEM からコンテンツを取得する web コンポーネントを作成する方法を説明します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/deployments/web-component#example-web-component" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## RESTful API - 例

[Assets HTTP API](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) や [JSON エクスポーター](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter)などの RESTful API の詳細を説明します。

### ユースケース

<!-- CARDS
{target = _self}

* https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview
  {title = Using Content Services for Headless App}
  {description = Learn how to build a native mobile app that fetches content from AEM using Content Services RESTful APIs.}
  {image = ./assets/RESTful-Content-Service.png}
* https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview
  {title = Token-based Authentication for RESTful APIs}
  {description = Learn how to invoke RESTful APIs using JSON Web Token (JWT) authentication.}
  {image = ./assets/RESTful-TokenAuth.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Using Content Services for Headless App">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" title="ヘッドレスアプリ用のコンテンツサービスの使用" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-Content-Service.png" alt="ヘッドレスアプリ用のコンテンツサービスの使用"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" title="ヘッドレスアプリ用のコンテンツサービスの使用">ヘッドレスアプリ用のコンテンツサービスの使用</a>
                    </p>
                    <p class="is-size-6">コンテンツサービス RESTful API を使用して、AEM からコンテンツを取得するネイティブモバイルアプリを作成する方法を説明します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Token-based Authentication for RESTful APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" title="RESTful API のトークンベースの認証" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/RESTful-TokenAuth.png" alt="RESTful API のトークンベースの認証"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" title="RESTful API のトークンベースの認証">RESTful API のトークンベースの認証</a>
                    </p>
                    <p class="is-size-6">JSON web トークン（JWT）認証を使用して RESTful API を呼び出す方法を説明します。</p>
                </div>
                <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


