---
title: AEM API の概要
description: Adobe Experience Manager（AEM）の様々なタイプの API について説明し、OpenAPI 仕様ベースの API、一般的に OpenAPI ベースのAEM API の概要を示します。
version: Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-16515
thumbnail: KT-16515.jpeg
last-substantial-update: 2024-11-20T00:00:00Z
duration: 0
exl-id: 23b2be0d-a8d4-4521-96ba-78b70f4e9cba
source-git-commit: 2b5f7a033921270113eb7f41df33444c4f3d7723
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 2%

---

# AEM API の概要{#aem-apis-overview}

Adobe Experience Manager（AEM）as a Cloud Serviceの様々なタイプの API について説明し、[OpenAPI 仕様（OAS） ](https://swagger.io/specification/) ベースのAEM API、一般的に OpenAPI ベースのAEM API の概要を示します。

AEM as a Cloud Serviceは、コンテンツ、アセットおよびフォームを作成、読み取り、更新および削除するための幅広い API を提供します。 これらの API により、デベロッパーはAEMとやり取りするカスタムアプリケーションを作成できます。

AEMの様々なタイプの API を調べて、AdobeAPI へのアクセスの主要な概念を理解しましょう。

## AEM API の種類{#types-of-aem-apis}

AEMは、オーサーサービスとパブリッシュサービスのタイプとやり取りするための従来の API と最新の API の両方を提供しています。

- **レガシー API**：以前のAEM バージョンで導入されたレガシー API は、後方互換性のために引き続きサポートされています。

- **最新の API**:REST、OpenAPI 仕様に基づき、これらの API は、現在の API デザインのベストプラクティスに従っており、新しい統合に推奨されます。


| AEM API タイプ | 仕様 | 入手方法 | ユースケース | 例 |
| --- | --- | --- | --- | --- |
| 従来の（非 RESTful） API | Sling サーブレット | AEM 6.X、AEM as a Cloud Service | レガシー統合、下位互換性 | [Query Builder API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/search/query-builder-api) その他 |
| RESTful API | HTTP、JSON | AEM 6.X、AEM as a Cloud Service | CRUD 操作、最新のアプリケーション | [Assets HTTP API](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets)、[ ワークフロー REST API](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/implementing/developing/extending-aem/extending-workflows/workflows-program-interaction#using-the-workflow-rest-api)、[ コンテンツサービス用の JSON エクスポーター ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/full-stack/components-templates/json-exporter) など |
| GraphQLの API | GraphQL | AEM 6.X、AEM as a Cloud Service | ヘッドレス CMS、SPA、モバイルアプリ | [GraphQL API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/content-fragments) |
| OpenAPI ベースのAEM API | REST、OpenAPI | **AEM as a Cloud Serviceのみ** | API ファースト開発、最新のアプリケーション | [Assets オーサー API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/)、[Folders API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)、[AEM Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/)、[Forms Acrobat サービス ](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/) その他 |

>[!IMPORTANT]
>
>OpenAPI ベースのAEM API はAEM as a Cloud Serviceでのみ使用でき、AEM 6.X とは互換性がありません。

AEM API について詳しくは、[Adobe Experience Manager as a Cloud Service API](https://developer.adobe.com/experience-cloud/experience-manager-apis/) を参照してください。

OpenAPI ベースのAEM API と、AdobeAPI へのアクセスの重要な概念を詳しく見てみましょう。

## OpenAPI ベースのAEM API{#openapi-based-aem-apis}

>[!AVAILABILITY]
>
>OpenAPI ベースのAEM API は、早期アクセスプログラムの一部として利用できます。 これらにアクセスすることに関心がある場合は、ユースケースの説明を記載した電子メール ](mailto:aem-apis@adobe.com)0}aem-apis@adobe.com} を送信することをお勧めします。[

[OpenAPI 仕様 ](https://swagger.io/specification/) （旧称 Swagger）は、RESTful API の定義に広く使用されている標準です。 AEM as a Cloud Serviceには、複数の OpenAPI 仕様ベースの API （または単に OpenAPI ベースのAEM API）が用意されており、AEMのオーサーサービスまたはパブリッシュサービスのタイプとやり取りするカスタムアプリケーションを簡単に作成できます。 以下に例を示します。

**Sites**

- [Sites API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/sites/delivery/)：コンテンツフラグメントを操作するための API。

**Assets**

- [ フォルダー API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/folders/)：フォルダーの作成、リスト化、削除などのフォルダーを操作するための API。

- [Assets オーサー API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/): アセットとそのメタデータを操作するための API。

**Forms**

- [Forms通信 API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/document/): フォームおよびドキュメントを操作するための API。

今後のリリースでは、その他のユースケースをサポートするために、さらに OpenAPI ベースのAEM API が追加される予定です。

### 認証のサポート{#authentication-support}

OpenAPI ベースのAEM API は、次の認証方法をサポートしています。

- **OAuth サーバー間資格情報**: ユーザーの操作なしで API へのアクセスが必要なバックエンドサービスに最適です。 _client_credentials_ 付与タイプを使用して、サーバーレベルで安全なアクセス管理を有効にします。 詳しくは、[OAuth サーバー間資格情報 ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/#oauth-server-to-server-credential) を参照してください。

- **OAuth web アプリ資格情報**：ユーザーの代わりにAEM API にアクセスするフロントエンドおよび _バックエンド_ コンポーネントを持つ web アプリケーションに適しています。 _authorization_code_ 付与タイプを使用し、バックエンドサーバーが秘密鍵とトークンを安全に管理します。 詳しくは、[OAuth Web アプリ資格情報 ](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-web-app-credential) を参照してください。

- **OAuth シングルページアプリ資格情報**：ブラウザーで動作するSPA用に設計されています。このブラウザーは、バックエンドサーバーを使用せずに、ユーザーの代わりに API にアクセスする必要があります。 _authorization_code_ 付与タイプを使用し、PKCE （Proof Key for Code Exchange）を使用したクライアント側のセキュリティメカニズムに依存して、認証コードフローを保護します。 詳しくは、[OAuth 単一ページアプリ資格情報 ](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation/#oauth-single-page-app-credential) を参照してください。

### OAuth サーバー間と OAuth web アプリ/シングルページアプリの資格情報の違い{#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials}

| | OAuth サーバー間 | OAuth ユーザー認証（web アプリ） |
| --- | --- | --- |
| 認証の目的 | 機械間インタラクション用に設計されています。 | ユーザー主導のインタラクション用に設計されています。 |
| トークンの動作 | クライアントアプリケーション自体を表すアクセストークンを発行します。 | 認証済みユーザーの代わりにアクセストークンを発行します。 |
| ユースケース | ユーザーインタラクションのない API アクセスを必要とするバックエンドサービス。 | ユーザーの代わりに API にアクセスする、フロントエンドおよびバックエンドコンポーネントを持つ web アプリケーション。 |
| セキュリティに関する考慮事項 | 機密性の高い資格情報（`client_id`、`client_secret`）をバックエンドシステムに安全に保存します。 | ユーザーの認証をおこなうと、独自の一時的なアクセストークンが付与されます。 機密性の高い資格情報（`client_id`、`client_secret`）をバックエンドシステムに安全に保存します。 |
| 付与タイプ | _client_credentials_ | _authorization_code_ |

### AdobeAPI および関連概念へのアクセス{#accessing-adobe-apis-and-related-concepts}

AdobeAPI にアクセスする前に、次の主要な概念を理解することが不可欠です。

- **[Adobe Developer Console](https://developer.adobe.com/)**:AdobeAPI、SDK、リアルタイムイベント、サーバーレス関数などにアクセスするための開発者ハブです。 AEM アプリケーションのデバッグに使用される _AEM_ Developer Consoleとは異なることに注意してください。

- **[Adobe Developer Console プロジェクト ](https://developer.adobe.com/developer-console/docs/guides/projects/)**: API 統合、イベントおよびランタイム関数を一元的に管理する場所です。 ここでは、API を設定し、認証を設定して、必要な資格情報を生成します。

- **[製品プロファイル ](https://helpx.adobe.com/jp/enterprise/using/manage-product-profiles.html)**：製品プロファイルは、AEM、Adobe Target、Adobe AnalyticsなどのAdobe製品へのユーザーまたはアプリケーションのアクセスを制御できる権限プリセットを提供します。 すべてのAdobe製品には、事前に定義された製品プロファイルが関連付けられています。

- **サービス**：サービスは、実際の権限を定義し、製品プロファイルに関連付けられます。 権限プリセットを減らしたり増やしたりするには、製品プロファイルに関連付けられているサービスの選択を解除または選択します。 これにより、製品とその API へのアクセスレベルを制御できます。 AEM as a Cloud Serviceでは、サービスは、リポジトリノード用に事前定義されたアクセス制御リスト（ACL）を持つユーザーグループを表し、詳細な権限管理を可能にします。

## 次の手順{#next-steps}

次のような様々なAEM API タイプを理解している場合
OpenAPI ベースのAEM API と、AdobeAPI へのアクセスに関する主な概念を理解したら、AEMとやり取りするカスタムアプリケーションの構築を開始できます。

それでは、始めましょう。

- [ サーバー間認証のための OpenAPI ベースのAEM API の呼び出し ](invoke-openapi-based-aem-apis.md) チュートリアル。このチュートリアルでは、（OAuth サーバー間資格情報を使用して _OpenAPI ベースのAEM API にアクセスする方法を実演_ します。
- [Web アプリからのユーザー認証を使用して OpenAPI ベースのAEM API を呼び出す ](invoke-openapi-based-aem-apis-from-web-app.md) チュートリアル。このチュートリアルでは、_Web アプリケーションから OAuth Web アプリ資格情報を使用して OpenAPI ベースのAEM API にアクセスする方法を説明します_。
