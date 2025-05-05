---
title: OpenAPI ベースのAEM API
description: 認証のサポート、主要概念、AEM API へのアクセス方法など、OpenAPI ベースのAdobe API について説明します。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 0eb0054d-0c0a-4ac0-b7b2-fdaceaa6479b
source-git-commit: 58ae9e503bd278479d78d4df6ffe39356d5ec59b
workflow-type: tm+mt
source-wordcount: '1100'
ht-degree: 2%

---

# OpenAPI ベースのAEM API

>[!IMPORTANT]
>
>OpenAPI ベースのAEM API はAEM as a Cloud Serviceでのみ使用でき、AEM 6.X とは互換性がありません。

認証のサポート、主要概念、AEM API へのアクセス方法など、OpenAPI ベースのAdobe API について説明します。

[OpenAPI 仕様 ](https://swagger.io/specification/) （旧称 Swagger）は、RESTful API の定義に広く使用されている標準です。 AEM as a Cloud Serviceには、複数の OpenAPI 仕様ベースの API （または単に OpenAPI ベースのAEM API）が用意されており、AEMのオーサーサービスまたはパブリッシュサービスと対話するカスタムアプリケーションを簡単に作成できます。 以下に例を示します。

**Sites**

- [Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/)：コンテンツフラグメントを操作するための API。

**Assets**

- [ フォルダー API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)：フォルダーの作成、リスト化、削除などのフォルダーを操作するための API。

- [Assets オーサー API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): アセットとそのメタデータを操作するための API。

**Forms**

- [Forms通信 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): フォームおよびドキュメントを操作するための API。

今後のリリースでは、追加のユースケースをサポートするために、OpenAPI ベースのAEM API がさらに追加される予定です。

## 認証のサポート{#authentication-support}

OpenAPI ベースのAEM API は、次の付与タイプを含む、OAuth 2.0 認証をサポートしています。

- **OAuth サーバー間資格情報**: ユーザーの操作なしで API へのアクセスが必要なバックエンドサービスに最適です。 _client_credentials_ 付与タイプを使用して、サーバーレベルで安全なアクセス管理を有効にします。 詳しくは、[OAuth サーバー間資格情報 ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential) を参照してください。

- **OAuth web アプリ資格情報**：ユーザーの代わりにAEM API にアクセスするフロントエンドおよび _バックエンド_ コンポーネントを持つ web アプリケーションに適しています。 _authorization_code_ 付与タイプを使用し、バックエンドサーバーが秘密鍵とトークンを安全に管理します。 詳しくは、[OAuth Web アプリ資格情報 ](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) を参照してください。

- **OAuth 単一ページアプリ資格情報**：ブラウザーで実行される SPA 用に設計されています。バックエンドサーバーを使用せずに、ユーザーの代わりに API にアクセスする必要があります。 _authorization_code_ 付与タイプを使用し、PKCE （Proof Key for Code Exchange）を使用したクライアント側のセキュリティメカニズムに依存して、認証コードフローを保護します。 詳しくは、[OAuth 単一ページアプリ資格情報 ](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) を参照してください。

## 使用する認証方法{#auth-method-decision}

使用する認証方法を決定する際は、次の点を考慮してください。

![ 使用する認証方法 ](./assets/overview/which-authentication-method-to-use.png)

AEM ユーザーコンテキストが関係する場合は常に、ユーザー認証（web アプリまたはシングルページアプリ）がデフォルトの選択になります。 これにより、リポジトリ内のすべてのアクションが認証済みユーザーに適切に関連付けられ、ユーザーは権限のある権限のみに制限されます。
個々のユーザーの代わりにサーバー間（または技術的なシステムアカウント）でアクションを実行すると、セキュリティモデルがバイパスされ、権限のエスカレーションや不正確な監査などのリスクが発生します。

## OAuth サーバー間と web アプリとシングルページアプリの資格情報の違い{#difference-between-oauth-server-to-server-vs-web-app-vs-single-page-app-credentials}

次の表に、OpenAPI ベースのAEM API でサポートされる 3 つの OAuth 認証方法の違いを示します。

|  | OAuth サーバー間 | OAuth Web アプリ | OAuth 単一ページアプリ（SPA） |
| --- | --- | --- | --- |
| **認証の目的** | 機械間インタラクション用に設計されています。 | _バックエンド_ を使用した web アプリでのユーザー主導のインタラクション用に設計されています。 | _クライアントサイド JavaScript アプリケーション_ でのユーザー主導のインタラクション用に設計されています。 |
| **トークン動作** | クライアントアプリケーション自体を表すアクセストークンを発行します。 | 認証済みユーザーの代わりに _バックエンド経由で_ アクセストークンを発行します。 | 認証済みユーザーの代わりに _フロントエンドのみのフローを介して_ アクセストークンを発行します。 |
| **ユースケース** | ユーザーインタラクションのない API アクセスを必要とするバックエンドサービス。 | ユーザーの代わりに API にアクセスする、フロントエンドおよびバックエンドコンポーネントを持つ web アプリケーション。 | バックエンドを持たないユーザーの代わりに API にアクセスする、純粋なフロントエンド（JavaScript）アプリケーション。 |
| **セキュリティに関する考慮事項** | 機密性の高い資格情報（`client_id`、`client_secret`）をバックエンドシステムに安全に保存します。 | ユーザー認証後、ユーザーには、バックエンド呼び出しを使用して独自の _一時的なアクセストークン_ が付与されます。 アクセストークンの認証コードを交換するために、機密性の高い資格情報（`client_id`、`client_secret`）をバックエンドシステムに安全に保存します。 | ユーザー認証後、ユーザーには、フロントエンド呼び出しを通じて独自の _一時的アクセストークン_ が付与されます。 フロントエンドアプリに保存すると安全ではないため、`client_secret` は使用しません。 アクセストークンの認証コードを交換するために PKCE に依存します。 |
| **付与タイプ** | _client_credentials_ | _authorization_code_ | _authorization_code_ と **PKCE** |
| **Adobe Developer Consoleの資格情報の種類** | OAuth サーバー間 | OAuth Web アプリ | OAuth 単一ページアプリ |
| **チュートリアル** | [ サーバー間認証を使用した API の呼び出し ](./use-cases/invoke-api-using-oauth-s2s.md) | [Web アプリ認証を使用した API の呼び出し ](./use-cases/invoke-api-using-oauth-web-app.md) | [ シングルページアプリ認証を使用した API の呼び出し ](./use-cases/invoke-api-using-oauth-single-page-app.md) |

## Adobe API へのアクセスと関連概念{#accessing-adobe-apis-and-related-concepts}

Adobe API にアクセスする前に、次の主要な構成要素を理解しておく必要があります。

- **[Adobe Developer Console](https://developer.adobe.com/)**: Adobe API、SDK、リアルタイムイベント、サーバーレス関数などにアクセスするための開発者ハブです。 これは、AEM アプリケーションのデバッグに使用される _0&rbrace;Developer Console&rbrace;AEMとは異なることに注意してください。_

- **[Adobe Developer Console プロジェクト ](https://developer.adobe.com/developer-console/docs/guides/projects/)**: API 統合、イベントおよびランタイム関数を一元的に管理する場所です。 ここでは、API を設定し、認証を設定して、必要な資格情報を生成します。

- **[製品プロファイル ](https://helpx.adobe.com/jp/enterprise/using/manage-product-profiles.html)**：製品プロファイルには、AEM、Adobe Target、Adobe AnalyticsなどのAdobe製品へのユーザーまたはアプリケーションのアクセスを制御できる権限プリセットが用意されています。 すべてのAdobe製品には、事前に定義された製品プロファイルが関連付けられています。

- **サービス**：サービスは、実際の権限を定義し、製品プロファイルに関連付けられます。 権限プリセットを減らしたり増やしたりするには、製品プロファイルに関連付けられているサービスの選択を解除または選択します。 これにより、製品とその API へのアクセスレベルを制御できます。 AEM as a Cloud Serviceでは、サービスは、リポジトリノード用に事前定義されたアクセス制御リスト（ACL）を持つユーザーグループを表し、詳細な権限管理を可能にします。

## 今すぐ始める

AEM as a Cloud Service環境とAdobe Developer Console プロジェクトを設定して、OpenAPI ベースのAEM API へのアクセスを有効にする方法を説明します。 また、ブラウザーを使用してAEM API にアクセスし、設定を確認して、リクエストと応答を確認します。

<!-- CARDS
{target = _self}

* ./setup.md
  {title = Set up OpenAPI-based AEM APIs}
  {description = Learn how to set up your AEM as a Cloud Service environment to enable access to the OpenAPI-based AEM APIs.}
  {image = ./assets/setup/OpenAPI-Setup.png}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up OpenAPI-based AEM APIs">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup.md" title="OpenAPI ベースのAEM API の設定" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/setup/OpenAPI-Setup.png" alt="OpenAPI ベースのAEM API の設定"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup.md" target="_self" rel="referrer" title="OpenAPI ベースのAEM API の設定">OpenAPI ベースのAEM API の設定 </a>
                    </p>
                    <p class="is-size-6">AEM as a Cloud Service環境を設定して、OpenAPI ベースのAEM API へのアクセスを有効にする方法について説明します。</p>
                </div>
                <a href="./setup.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->


## API チュートリアル

様々な OAuth 認証方法を使用して、OpenAPI ベースのAEM API を使用する方法について説明します。

<!-- CARDS
{target = _self}

* ./use-cases/invoke-api-using-oauth-s2s.md
  {title = Invoke API using Server-to-Server authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom NodeJS application using OAuth Server-to-Server authentication.}
  {image = ./assets/s2s/OAuth-S2S.png}
* ./use-cases/invoke-api-using-oauth-web-app.md
  {title = Invoke API using Web App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom web application using OAuth Web App authentication.}
  {image = ./assets/web-app/OAuth-WebApp.png}
* ./use-cases/invoke-api-using-oauth-single-page-app.md
  {title = Invoke API using Single Page App authentication}
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth Single Page App authentication.}
  {image = ./assets/spa/OAuth-SPA.png}  
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Server-to-Server authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-s2s.md" title="サーバー間認証を使用した API の呼び出し" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/s2s/OAuth-S2S.png" alt="サーバー間認証を使用した API の呼び出し"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" title="サーバー間認証を使用した API の呼び出し"> サーバー間認証を使用した API の呼び出し </a>
                    </p>
                    <p class="is-size-6">OAuth サーバー間認証を使用して、カスタム NodeJS アプリケーションから OpenAPI ベースのAEM API を呼び出す方法を説明します。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-s2s.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Web App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-web-app.md" title="Web アプリ認証を使用した API の呼び出し" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/web-app/OAuth-WebApp.png" alt="Web アプリ認証を使用した API の呼び出し"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" title="Web アプリ認証を使用した API の呼び出し">Web アプリ認証を使用した API の呼び出し </a>
                    </p>
                    <p class="is-size-6">OAuth web アプリ認証を使用して、カスタム web アプリケーションから OpenAPI ベースのAEM API を呼び出す方法を説明します。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-web-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Invoke API using Single Page App authentication">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" title="シングルページアプリ認証を使用した API の呼び出し" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/spa/OAuth-SPA.png" alt="シングルページアプリ認証を使用した API の呼び出し"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" title="シングルページアプリ認証を使用した API の呼び出し"> シングルページアプリ認証を使用した API の呼び出し </a>
                    </p>
                    <p class="is-size-6">OAuth 単一ページアプリ認証を使用して、カスタム単一ページアプリ（SPA）から OpenAPI ベースのAEM API を呼び出す方法を説明します。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
