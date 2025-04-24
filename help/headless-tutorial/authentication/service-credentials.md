---
title: サービス資格情報
description: 外部のアプリケーション、システム、サービスを容易にするために使用されるサービス資格情報を使用して、HTTP 経由でオーサーサービスまたはパブリッシュサービスとプログラムでやり取りする方法について説明します。
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
duration: 881
source-git-commit: bb4f9982263a15f18b9f39b1577b61310dfbe643
workflow-type: tm+mt
source-wordcount: '1963'
ht-degree: 99%

---

# サービス資格情報

Adobe Experience Manager（AEM）as a Cloud Service との統合については、AEM サービスに対して確実な認証を行う必要があります。 AEM の Developer Console は、外部のアプリケーション、システムおよびサービスが HTTP 経由で AEM オーサーまたはパブリッシュサービスとプログラムでやり取りするのを容易にするために使用するサービス資格情報へのアクセス権を付与します。

AEM は、[Adobe Developer Console で管理される S2S OAuth](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/setting-up-ims-integrations-for-aem-as-a-cloud-service) を使用して、他のアドビ製品と統合されています。サービスアカウントとのカスタム統合の場合、JWT 資格情報が AEM Developer Console で使用および管理されます。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

サービス資格情報が[ローカル開発アクセストークン](./local-development-access-token.md)と似ているように見えるかもしれませんが、いくつかの重要な点で異なります。

+ サービス資格情報はテクニカルアカウントに関連付けられています。 1 つのテクニカルアカウントに対して複数のサービス資格情報をアクティブにできます。
+ サービス資格情報はアクセストークン&#x200B;_ではなく_、アクセストークンの&#x200B;_取得_&#x200B;に使用されます。
+ サービス資格情報はより永続的です（証明書は 365 日ごとに有効期限が切れます）。ローカル開発アクセストークンは毎日期限切れになるのに対して、削除しない限り変更されることはありません。
+ AEM as a Cloud Service 環境のサービス資格情報は、単一の AEM テクニカルアカウントユーザーにマッピングされます。ローカル開発アクセストークンは、アクセストークンを生成した AEM ユーザーとして認証されます。
+ AEM as a Cloud Service 環境は、最大 10 個のテクニカルアカウント（それぞれに独自のサービス資格情報）を持つことができ、それぞれがテクニカルアカウントの個別の AEM ユーザーに割り当てられます。

生成するサービス資格情報とアクセストークン、およびローカル開発アクセストークンは、両方とも秘密にする必要があります。3 つすべてを使用して取得し、それぞれの AEM as a Cloud Service 環境にアクセスします。

## サービス資格情報の生成

サービス資格情報の生成は、次の 2 つの手順に分かれます。

1. Adobe IMS 組織管理者による 1 回限りのテクニカルアカウントの作成
1. テクニカルアカウントのサービス資格情報 JSON のダウンロードおよび使用

### テクニカルアカウントの作成

ローカル開発アクセストークンとは異なり、サービス資格情報はダウンロードする前に、アドビ組織 IMS 管理者がテクニカルアカウントを作成する必要があります。 AEM にプログラムでアクセスする必要があるクライアントごとに、個別のテクニカルアカウントを作成する必要があります。

![テクニカルアカウントの作成](assets/service-credentials/initialize-service-credentials.png)

テクニカルアカウントは 1 回だけ作成されますが、秘密鍵はテクニカルアカウントに関連付けられたサービス資格情報の管理に使用します。 例えば、新しい秘密鍵とサービス資格情報は、現在の秘密鍵の有効期限が切れる前に生成され、サービス資格情報のユーザーが中断なしにアクセスできるようにする必要があります。

1. 必ず以下としてログインします。
   + __Adobe IMS 組織のシステム管理者__
   + __AEM オーサー__&#x200B;の __AEM 管理者__ IMS 製品プロファイルのメンバー
1. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) にログインします。 
1. AEM as a Cloud Service 環境を含むプログラムを開き、サービス資格情報のセットアップを統合します。
1. 「__環境__」セクションの環境の横にある省略記号をタップし、「__Developer Console__」を選択します。
1. 「__統合__」タブをタップします。
1. 「__テクニカルアカウント__」タブをタップします。
1. 「__テクニカルアカウントを新規作成__」ボタンをタップします。
1. テクニカルアカウントのサービス資格情報が初期化され、JSON 形式で表示されます

![AEM Developer Console - 統合 - サービス資格情報の取得](./assets/service-credentials/developer-console.png)

AEM as a Cloud Service 環境のサービス資格情報が初期化されると、Adobe IMS 組織内の他の AEM 開発者がダウンロードできるようになります。

### サービス資格情報のダウンロード

![サービス資格情報のダウンロード](assets/service-credentials/download-service-credentials.png)

サービス資格情報のダウンロードは、初期化と同様の手順に従います。

1. 必ず以下としてログインします。
   + __Adobe IMS 組織管理者__
   + __AEM オーサー__&#x200B;の __AEM 管理者__ IMS 製品プロファイルのメンバー
1. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) にログインします。
1. 統合先の AEM as a Cloud Service 環境を含んだプログラムを開きます。
1. 「__環境__」セクションの環境の横にある省略記号をタップし、「__Developer Console__」を選択します。
1. 「__統合__」タブをタップします。
1. 「__テクニカルアカウント__」タブをタップします。 
1. 使用する&#x200B;__テクニカルアカウント__&#x200B;を展開します。
1. そのサービス資格情報がダウンロードされる&#x200B;__秘密鍵__&#x200B;を展開し、ステータスが「__アクティブ__」になっていることを確認します。
1. __秘密鍵__&#x200B;に関連づけられている __...__／__表示__&#x200B;をタップし、サービス資格情報 JSON を表示します。
1. 左上のダウンロードボタンをタップして、サービス資格情報の値を含む JSON ファイルをダウンロードし、安全な場所に保存します。

## サービス資格情報のインストール

JWT の生成に必要な詳細はサービス資格情報で指定されます。JWT は、AEM as a Cloud Service での認証に使用されるアクセストークンと交換されます。サービス資格情報は、AEM へのアクセスに使用する外部アプリケーション、システム、またはサービスがアクセスできる安全な場所に保存する必要があります。サービス資格情報の管理方法と管理場所は、顧客ごとに異なります。

簡単にするため、このチュートリアルではコマンドラインからサービス資格情報を渡します。ただし、IT セキュリティチームと協力して、組織のセキュリティガイドラインに従ってこれらの資格情報を保存し、アクセスする方法を理解するようにしてください。

1. [ダウンロードしたサービス資格情報 JSON ](#download-service-credentials)をプロジェクトのルートにある `service_token.json` という名前のファイルにコピーします。
   + _資格情報_ は Git にコミットしないようにしてください。

## サービス資格情報の使用

サービス資格情報（完全な JSON オブジェクト形式）は、JWT やアクセストークンとは異なります。秘密鍵を含むサービス資格情報を使用して JWT を生成し、アクセストークン用の Adobe IMS API と交換します。

![サービス資格情報 - 外部アプリケーション](assets/service-credentials/service-credentials-external-application.png)

1. AEM Developer Console から安全な場所にサービス資格情報をダウンロードします。
1. 外部アプリケーションは、AEM as a Cloud Service 環境とプログラムでやり取りする必要があります
1. 外部アプリケーションは、安全な場所からサービス資格情報を読み取ります。
1. 外部アプリケーションは、サービス資格情報の情報を使用して JWT トークンを構築します。
1. JWT トークンは Adobe IMS に送信され、アクセストークンと交換されます。
1. Adobe IMS は、AEM as a Cloud Service へのアクセスに使用できるアクセストークンを返します
   + アクセストークンの有効期限を変更することはできません。
1. 外部アプリケーションは、AEM as a Cloud Service に HTTP リクエストを行い、アクセストークンを Bearer トークンとして HTTP リクエストの認証ヘッダーに追加します。
1. AEM as a Cloud Service が、HTTP リクエストを受信して認証し、HTTP リクエストで要求された作業を実行し、HTTP レスポンスを外部アプリケーションに返します。

### 外部アプリケーションの更新

サービス資格情報を使用してAEM as a Cloud Service にアクセスするには、外部アプリケーションを次の 3 つの方法で更新する必要があります。

1. サービス資格情報を読み取る

+ 簡単にするために、サービス資格情報はダウンロードした JSON ファイルから読み取りますが、実際のシナリオでは組織のセキュリティガイドラインに従って、サービス資格情報を安全に保存してください。

1. サービス資格情報から JWT を生成する
1. JWT をアクセストークンと交換する

+ サービス資格情報が存在する場合、外部アプリケーションは AEM as a Cloud Service にアクセスする際に、ローカル開発アクセストークンではなく、このアクセストークンを使用します

このチュートリアルでは、（1）サービス資格情報からの JWT の生成、（2）1 回の関数呼び出しによるアクセストークンとの交換の両方にアドビの `@adobe/jwt-auth` npm モジュールを使用しています。アプリケーションが JavaScript ベースではない場合は[他の言語のサンプルコード](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples/)を参照し、サービス資格情報から JWT の作成方法と Abode IMS でのアクセストークンとの交換方法をご確認ください。

## サービス資格情報の読み取り

ローカル開発アクセストークン JSON で読み取るのと同じコードを使用して、サービス資格情報 JSON ファイルがどのように読み取られるかを `getCommandLineParams()` で確認します。

```javascript
function getCommandLineParams() {
    ...

    // Read in the credentials from the provided JSON file
    // Since both the Local Development Access Token and Service Credentials files are JSON, this same approach can be re-used
    if (parameters.file) {
        parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
    }

    ...
    return parameters;
}
```

## JWT の作成とアクセストークンとの交換

サービス資格情報が読み取られると、それらを使用して JWT が生成され、JWT がアクセストークン用の Adobe IMS API と交換されます。その後、このアクセストークンを使用して、AEM as a Cloud Service にアクセスできます。

このサンプルアプリケーションは Node.js ベースなので、[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm モジュールを使用すると、JWT の生成と Adobe IMS との交換がスムーズになります。アプリケーションが別の言語で開発されている場合は、[適切なコードサンプル](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/samples)を参照し、他のプログラミング言語を使用して Adobe IMS に対する HTTP リクエストを作成する方法を確認してください。

1. `getAccessToken(..)` を更新して JSON ファイルの内容を調べ、ローカル開発のアクセストークンかサービス資格情報かを判断します。その簡単な方法は、`.accessToken` プロパティが存在するかどうかを確認することです。このプロパティはローカル開発のアクセストークン JSON に対してのみ存在します。

   サービス資格情報が指定されている場合、アプリケーションは JWT を生成し、アクセストークンの Adobe IMS と交換します。[@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) の `auth(...)` 関数を使用して JWT を生成し、1 回の関数呼び出しでアクセストークンと交換します。以下のコードで説明されているように、`auth(..)` メソッドのパラメーターは、サービス資格情報 JSON から利用可能な[特定の情報で構成される JSON オブジェクト](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)です。

```javascript
 async function getAccessToken(developerConsoleCredentials) {

     if (developerConsoleCredentials.accessToken) {
         // This is a Local Development access token
         return developerConsoleCredentials.accessToken;
     } else {
         // This is the Service Credentials JSON object that must be exchanged with Adobe IMS for an access token
         let serviceCredentials = developerConsoleCredentials.integration;

         // Use the @adobe/jwt-auth library to pass the service credentials generated a JWT and exchange that with Adobe IMS for an access token.
         // If other programming languages are used, please see these code samples: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md
         let { access_token } = await auth({
             clientId: serviceCredentials.technicalAccount.clientId, // Client Id
             technicalAccountId: serviceCredentials.id,              // Technical Account Id
             orgId: serviceCredentials.org,                          // Adobe IMS Org Id
             clientSecret: serviceCredentials.technicalAccount.clientSecret, // Client Secret
             privateKey: serviceCredentials.privateKey,              // Private Key to sign the JWT
             metaScopes: serviceCredentials.metascopes.split(','),   // Meta Scopes defining level of access the access token should provide
             ims: `https://${serviceCredentials.imsEndpoint}`,       // IMS endpoint used to obtain the access token from
         });

         return access_token;
     }
 }
```

    ローカル開発のアクセストークン JSON またはサービス資格情報 JSON のどちらファイルが、「file」コマンドラインパラメーターを介して渡されるかに応じて、アプリケーションがアクセストークンを取得します。
    
    サービス資格情報は 365 日ごとに期限切れになりますが、JWT と対応するアクセストークンは頻繁に期限切れになり、期限切れになる前に更新する必要があることに注意してください。これは、「refresh_token」[Adobe IMSが提供する ]（https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens）を使用して実行できます。

1. これらの変更を行った上で、サービス資格情報 JSON は AEM Developer Console からダウンロードされ、簡単にするために、この `index.js` と同じフォルダー内に `service_token.json` として保存されました。次に、コマンドラインパラメーター `file` を `service_token.json` に置き換えてアプリケーションを実行し、`propertyValue` を新しい値に更新すると、効果が AEM で明らかになります。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   端末への出力は次のようになります。

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   __403 - Forbidden__ 行には、AEM as a Cloud Service への HTTP API 呼び出しでのエラーが示されます。これらの 403 Forbidden エラーは、アセットのメタデータを更新しようとする際に発生します。

   この理由は、サービス資格情報から得られるアクセストークンでは、自動作成されたテクニカルアカウント AEM ユーザー（デフォルトで読み取りアクセスのみ可能）を使用して AEM へのリクエストを認証するからです。AEM へのアプリケーション書き込みアクセス権を付与するには、アクセストークンに関連付けられているテクニカルアカウント AEM ユーザーに対して AEM で権限を付与する必要があります。

## AEM でのアクセス権の設定

サービス資格情報から得られるアクセストークンでは、__寄稿者__ AEM ユーザーグループに属するテクニカルアカウント AEM ユーザーを使用します。

![サービス資格情報 - テクニカルアカウント AEM ユーザー](./assets/service-credentials/technical-account-user.png)

（アクセストークンを使用した最初の HTTP リクエストの後で）テクニカルアカウント AEM ユーザーが AEM に存在するようになると、この AEM ユーザーの権限は他の AEM ユーザーと同じように管理できます。

1. まず、AEM Developer Console でダウンロードしたサービス資格情報 JSON を開いて、テクニカルアカウントの AEM ログイン名を探し、`integration.email` の値（例：`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`）を特定します。
1. 対応する AEM 環境のオーサーサービスに AEM 管理者としてログインします。
1. __ツール__／__セキュリティ__／__ユーザー__&#x200B;に移動します。
1. 手順 1 で特定した&#x200B;__ログイン名__&#x200B;を持つ AEM ユーザーを探し、その&#x200B;__プロパティ__&#x200B;を開きます。
1. 「__グループ__」タブに移動して、__DAM ユーザー__&#x200B;グループ（アセットへの書き込みアクセス権を持つグループ）を追加します。
   + [AEM が提供するユーザーグループのリストを参照](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=ja#built-in-users-and-groups)して、最適な権限を取得するためのサービスユーザーを追加します。AEM が提供するユーザーグループが十分でない場合は、独自のグループを作成し、適切な権限を追加します。
1. 「__保存して閉じる__」をタップします。

アセットに対する書き込み権限を持つテクニカルアカウントが AEM で許可された状態で、アプリケーションを再実行します。

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd-shared/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

端末への出力は次のようになります。

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 変更内容の確認

1. 更新された AEM as a Cloud Service 環境にログインします（`aem` コマンドラインパラメーターで指定したホスト名と同じホスト名を使用）。
1. __アセット__／__ファイル__&#x200B;に移動します。
1. `folder` コマンドラインパラメーターで指定したアセットフォルダーに移動します（例：__WKND__／__en__／__Adventures__／__Napa Wine Tasting__）。
1. フォルダー内の任意のアセットの&#x200B;__プロパティ__&#x200B;を開きます。
1. 「__詳細__」タブに移動します。
1. 更新されたプロパティの値を確認します。例えば、__Copyright__ は更新された `metadata/dc:rights` JCR プロパティにマッピングされ、`propertyValue` パラメーターで指定した値（例：__WKND Restricted Use__）が反映されています。

![WKND Restricted Use メタデータの更新](./assets/service-credentials/asset-metadata.png)

## おめでとうございます。

これで、ローカル開発アクセストークンと実稼動対応済みのサービス間アクセストークンを使用して、AEM as a Cloud Service にプログラムでアクセスできるようになりました。
