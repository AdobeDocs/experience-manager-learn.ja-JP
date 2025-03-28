---
title: OpenAPI ベースのAEM API の設定
description: AEM as a Cloud Service環境を設定して、OpenAPI ベースのAEM API へのアクセスを有効にする方法について説明します。
version: Experience Manager as a Cloud Service
feature: Developing
topic: Development, Architecture, Content Management
role: Architect, Developer, Leader
level: Beginner
doc-type: Article
jira: KT-17426
thumbnail: KT-17426.jpeg
last-substantial-update: 2025-02-28T00:00:00Z
duration: 0
exl-id: 1df4c816-b354-4803-bb6c-49aa7d7404c6
source-git-commit: 52aad0b0e568ff7e4acd23742fc70f10b1dd14ee
workflow-type: tm+mt
source-wordcount: '1253'
ht-degree: 7%

---

# OpenAPI ベースのAEM API の設定

AEM as a Cloud Service環境を設定して、OpenAPI ベースのAEM API へのアクセスを有効にする方法について説明します。

>[!AVAILABILITY]
>
>OpenAPI ベースのAEM API は、早期アクセスプログラムの一部として利用できます。 これらにアクセスすることに関心がある場合は、ユースケースの説明を記載した電子メール ](mailto:aem-apis@adobe.com)0}aem-apis@adobe.com} を送信することをお勧めします。[

の高度な設定プロセスには、次の手順が含まれます。

1. AEM as a Cloud Service環境の最新化。
1. AEM API アクセスを有効にします。
1. Adobe Developer Console（ADC）プロジェクトを作成します。
1. ADC プロジェクトの設定
1. AEM インスタンスを設定して、ADC プロジェクト通信を有効にします。

## AEM as a Cloud Serviceの最新化

AEM as a Cloud Service環境の最新化は、次の手順を含む環境アクティビティごとに 1 回行われます。

- AEM リリース **2024.10.18459.20241031T210302Z** 以降に更新します。
- リリース 2024.10.18459.20241031T210302Z より前に環境が作成された場合は、新しい製品プロファイルを追加します。

### AEM インスタンスの更新

AEM インスタンスを更新するには、「Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/)」の「_環境_」セクションで、環境名の横にある _省略記号_ アイコンを選択し、「**更新**」オプションを選択します。

![AEM インスタンスの更新 ](./assets/setup/update-aem-instance.png)

次に、「**送信**」ボタンをクリックし、_推奨_ フルスタックパイプラインを実行します。

![ 最新のAEM リリースバージョンを選択 ](./assets/setup/select-latest-aem-release.png)

この場合、フルスタックパイプラインの名前は **Dev :: Fullstack-Deploy** で、AEM環境の名前は **wknd-program-dev** です。 名前が異なる場合があります。

### 新しい製品プロファイルを追加

AEM インスタンスに新しい製品プロファイルを追加するには、Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) の「_環境_」セクションで、環境名の横にある _省略記号_ アイコンを選択し、「**製品プロファイルを追加**」オプションを選択します。

![ 新しい製品プロファイルを追加 ](./assets/setup/add-new-product-profiles.png)

環境名の横にある _省略記号_ アイコンをクリックし、**アクセスを管理**/**プロファイルを作成** を選択すると、新しく追加された製品プロファイルを確認できます。

_Admin Console_ ウィンドウに、新しく追加された製品プロファイルが表示されます。

![ 新製品プロファイルの確認 ](./assets/setup/review-new-product-profiles.png)

上記の手順で、AEM as a Cloud Service環境の最新化が完了します。

## AEM API アクセスの有効化

_新しい製品プロファイル_ が存在すると、Adobe Developer Console（ADC）で OpenAPI ベースのAEM API アクセスが可能になります。 [Adobe Developer Console（ADC） ](./overview.md#accessing-adobe-apis-and-related-concepts) は、Adobe API、SDK、リアルタイムイベント、サーバーレス関数などにアクセスするための開発者ハブです。

新しく追加された製品プロファイルは、_サービス_ に関連付けられています。これは、事前定義されたアクセス制御リスト（ACL） _を持つ_AEM ユーザーグループを表します。
_サービス_ は、AEM API へのアクセスレベルを制御するために使用されます。

また、製品プロファイルに関連付けられた _サービス_ を選択または選択解除して、アクセスレベルを増減することもできます。

製品プロファイル名の横にある _詳細を表示_ アイコンをクリックして、関連付けを確認します。

![ 製品プロファイルに関連付けられたサービスのレビュー ](./assets/setup/review-services-associated-with-product-profile.png)

デフォルトでは、**AEM Assets API Users** Service は製品プロファイルに関連付けられていません。 新しく追加された **AEM Assets Collaborator Users - author - Program XXX - Environment XXX** Product Profile に関連付けましょう。 この関連付けの後、ADC プロジェクトの _Asset Author API_ で目的の認証を設定し、認証アカウントを製品プロファイルに関連付けることができます。

![AEM Assets API Users Service と製品プロファイルの関連付け ](./assets/setup/associate-aem-assets-api-users-service-with-product-profile.png)

## Adobe Developer Console（ADC）プロジェクトの作成

ADC プロジェクトを使用して、目的の API を追加し、その認証を設定して、認証アカウントを製品プロファイルに関連付けます。

ADC プロジェクトを作成するには、次の手順に従います。

1. Adobe IDを使用して ](https://developer.adobe.com/console)0}Adobe Developer Console} にログインします。[

   ![Adobe Developer Console](./assets/setup/adobe-developer-console.png)

1. 「_クイックスタート_」セクションで、「**新規プロジェクトを作成**」ボタンをクリックします。

   ![ 新規プロジェクトを作成 ](./assets/setup/create-new-project.png)

1. これにより、デフォルトの名前で新しいプロジェクトが作成されます。

   ![ 新しいプロジェクトが作成されました ](./assets/setup/new-project-created.png)

1. 右上隅の **プロジェクトを編集** ボタンをクリックして、プロジェクト名を編集します。 意味のある名前を指定し、「保存 **をクリックし** す。

   ![ プロジェクト名を編集 ](./assets/setup/edit-project-name.png)

## ADC プロジェクトの設定

ADC プロジェクトを作成したら、目的のAEM API を追加し、その認証を設定して、認証アカウントを製品プロファイルに関連付ける必要があります。

1. AEM API を追加するには、「**API を追加**」ボタンをクリックします。

   ![API を追加](./assets/s2s/add-api.png)

1. _API を追加_ ダイアログで、_Experience Cloudでフィルタリングし_ 目的のAEM API を選択します。 例えば、この場合、「_アセットオーサー API_ が選択されます。

   ![AEM API を追加 ](./assets/s2s/add-aem-api.png)

1. 次に、_API を設定_ ダイアログで、目的の認証オプションを選択します。 例えば、この場合、「**サーバー間**」認証オプションが選択されています。

   ![ 認証を選択 ](./assets/s2s/select-authentication.png)

   サーバー間の認証は、ユーザーの操作なしで API へのアクセスが必要なバックエンドサービスに最適です。 Web アプリとシングルページアプリの認証オプションは、ユーザーの代わりに API アクセスを必要とするアプリケーションに適しています。 詳しくは、[OAuth サーバー間資格情報と OAuth web アプリ/シングルページアプリ資格情報の違い ](./overview.md#difference-between-oauth-server-to-server-and-oauth-web-app-single-page-app-credentials) を参照してください。

1. 必要に応じて、識別しやすくするために API の名前を変更できます。 デモ用に、デフォルト名が使用されます。

   ![ 資格情報の名前を変更 ](./assets/s2s/rename-credential.png)

1. この場合、認証方法は **OAuth サーバー間** なので、認証アカウントを製品プロファイルに関連付ける必要があります。 「**AEM Assets Collaborator Users - author - Program XXX - Environment XXX** Product Profile」を選択し、「**保存**」をクリックします。

   ![製品プロファイルを選択](./assets/s2s/select-product-profile.png)

1. AEM API と認証設定を確認します。

   ![AEM API 設定 ](./assets/s2s/aem-api-configuration.png)

   ![ 認証設定 ](./assets/s2s/authentication-configuration.png)

**OAuth Web アプリ** または **OAuth 単一ページアプリ** の認証方法を選択した場合、製品プロファイルの関連付けは表示されませんが、アプリケーションリダイレクト URI が必要です。 アプリケーションリダイレクト URI は、認証コードによる認証後にユーザーをアプリケーションにリダイレクトするために使用されます。 関連するユースケースのチュートリアルでは、このような認証固有の設定の概要を説明します。

## AEM インスタンスを設定して、ADC プロジェクト通信を有効にします

ADC プロジェクトのクライアント ID をAEM インスタンスと通信できるようにするには、AEM インスタンスを設定する必要があります。

それには、の `config.yaml` ファイルで API 設定を定義します。
AEM プロジェクトをデプロイし、Cloud Managerで設定パイプラインを使用してデプロイする。

1. AEM プロジェクトで、`config` フォルダーから `config.yaml` ファイルを見つけるか作成します。

   ![YAML 設定ファイルを見つける](./assets/setup/locate-config-yaml.png)

1. 次の設定を `config.yaml` ファイルに追加します。

   ```yaml
   kind: "API"
   version: "1.0"
   metadata: 
       envTypes: ["dev", "stage", "prod"]
   data:
       allowedClientIDs:
           author:
           - "<ADC Project's Credentials ClientID>"
   ```

   `<ADC Project's Credentials ClientID>` を ADC プロジェクトの資格情報の値の実際の ClientID に置き換えます。 このチュートリアルで使用する API エンドポイントはオーサー層でのみ使用できますが、その他の API の場合、yaml 設定に _publish_ または _preview_ ノードを含めることもできます。

   >[!CAUTION]
   >
   > デモ目的では、すべての環境で同じクライアント ID が使用されます。セキュリティと制御の強化に、環境（開発、ステージ、実稼動）ごとに個別のクライアント ID を使用することをお勧めします。

1. 設定変更をコミットし、Cloud Manager パイプラインが接続されているリモート Git リポジトリに変更をプッシュします。

1. Cloud Manager の設定パイプラインを使用して、上記の変更をデプロイします。 また、`config.yaml` ファイルは、コマンドラインツールを使用して RDE にインストールすることもできます。

   ![config.yaml のデプロイ](./assets/setup/config-pipeline.png)

## 次の手順

AEM インスタンスを設定して ADC プロジェクト通信を有効にすると、OpenAPI ベースのAEM API の使用を開始できます。 様々な OAuth 認証方法を使用して、OpenAPI ベースのAEM API を使用する方法について説明します。

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
  {description = Learn how to invoke OpenAPI-based AEM APIs from a custom Single Page App (SPA) using OAuth 2.0 PKCE flow.}
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
                    <p class="is-size-6">OAuth 2.0 PKCE フローを使用して、カスタム単一ページアプリ（SPA）から OpenAPI ベースのAEM API を呼び出す方法を説明します。</p>
                </div>
                <a href="./use-cases/invoke-api-using-oauth-single-page-app.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">詳細情報</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
