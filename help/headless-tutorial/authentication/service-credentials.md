---
title: サービス資格情報
description: 外部のアプリケーション、システム、サービスを容易にするために使用されるサービス資格情報を使用して、HTTP 経由でオーサーサービスまたはパブリッシュサービスとプログラム的にやり取りする方法について説明します。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
exl-id: e2922278-4d0b-4f28-a999-90551ed65fb4
source-git-commit: ef11609fe6ab266102bdf767a149284b9b912f98
workflow-type: tm+mt
source-wordcount: '1895'
ht-degree: 0%

---

# サービス資格情報

Adobe Experience Manager(AEM)as a Cloud Serviceとの統合では、AEMサービスに対する安全な認証をおこなう必要があります。 AEM Developer Console は、外部のアプリケーション、システムおよびサービスが HTTP 経由で AEM オーサーまたはパブリッシュサービスとプログラム的にやり取りするのを容易にするために使用する、サービス資格情報へのアクセス権を付与します。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

サービス資格情報が似たように表示される場合があります [ローカル開発アクセストークン](./local-development-access-token.md) しかし、いくつかの主な方法で異なります。

+ サービス資格情報： _not_ アクセストークン。 _取得_ アクセストークン。
+ サービス資格情報はより永続的で（365 日ごとに期限切れ）、失効しない限り変更しません。ローカル開発アクセストークンは毎日期限切れになります。
+ AEMas a Cloud Service環境のサービス資格情報は、単一のAEMテクニカルアカウントユーザーにマッピングされます。ローカル開発アクセストークンは、アクセストークンを生成したAEMユーザーとして認証されます。
+ AEMas a Cloud Service環境には、1 人のテクニカルアカウントAEMユーザーにマッピングされる 1 つのサービス資格情報があります。 サービス資格情報を使用して、別のテクニカルアカウントのAEMユーザーと同じAEMas a Cloud Service環境に対する認証を行うことはできません。

生成するサービス資格情報とアクセストークン、およびローカル開発アクセストークンの両方を秘密にする必要があります。 3 つすべてを使用して取得できるので、それぞれのAEMas a Cloud Service環境にアクセスします。

## サービス資格情報の生成

サービス資格情報の生成は、次の 2 つの手順に分かれます。

1. Adobe IMS組織管理者による 1 回限りのサービス資格情報の初期化
1. サービス資格情報 JSON のダウンロードと使用

### サービス資格情報の初期化

サービス資格情報は、ローカル開発アクセストークンとは異なり、 _1 回限りの初期化_ AdobeOrg IMS 管理者に問い合わせてください。

![サービス資格情報の初期化](assets/service-credentials/initialize-service-credentials.png)

__これは、AEMas a Cloud Service環境ごとに 1 回限りの初期化です__

1. 次のようにしてログインしていることを確認します。
   + Adobe IMS組織の管理者
   + のメンバー __Cloud Manager — 開発者__ IMS 製品プロファイル
   + のメンバー __AEM User__ または __AEM Administrators__ IMS 製品プロファイル ( ) __AEM オーサー__
1. にログインします。 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. AEMas a Cloud Service環境を含むプログラムを開き、のサービス資格情報を設定できるようにします。
1. 内の環境の横にある省略記号をタップします。 __環境__ セクションを選択し、 __開発者コンソール__
1. 次の __統合__ タブ
1. タップ __サービス資格情報の取得__ ボタン
1. サービス資格情報が初期化され、JSON として表示されます

![AEM開発者コンソール — 統合 — サービス資格情報の取得](./assets/service-credentials/developer-console.png)

AEM as aCloud Service環境のサービス資格情報が初期化されると、Adobe IMS組織内の他のAEM開発者がそれらをダウンロードできます。

### サービス資格情報のダウンロード

![サービス資格情報のダウンロード](assets/service-credentials/download-service-credentials.png)

サービス資格情報のダウンロードは、初期化と同じ手順に従います。 初期化がまだおこなわれていない場合は、 __サービス資格情報の取得__ 」ボタンをクリックします。

1. 次の形式でログインしていることを確認します。
   + のメンバー __Cloud Manager — 開発者__ IMS 製品プロファイル (AEM Developer Console へのアクセス権を付与 )
      + Sandbox AEMas a Cloud Service環境では、これは必要ありません  __Cloud Manager — 開発者__ メンバーシップ
   + のメンバー __AEM User__ または __AEM Administrators__ IMS 製品プロファイル ( ) __AEM オーサー__
1. にログインします。 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 統合するAEMas a Cloud Service環境を含むプログラムを開きます。
1. 内の環境の横にある省略記号をタップします。 __環境__ セクションを選択し、 __開発者コンソール__
1. 次の __統合__ タブ
1. タップ __サービス資格情報の取得__ ボタン
1. 左上隅のダウンロードボタンをタップして、「サービス資格情報」の値を含む JSON ファイルをダウンロードし、安全な場所に保存します。

+ _サービス資格情報に問題が発生した場合は、ただちにAdobeサポートに連絡し、失効させてもらってください_

## サービス資格情報のインストール

JWT の生成に必要な詳細はサービス資格情報で指定されます。JWT は、AEM as a Cloud Serviceでの認証に使用されるアクセストークンと交換されます。 サービス資格情報は、AEMへのアクセスに使用する外部アプリケーション、システム、またはサービスがアクセスできる安全な場所に保存する必要があります。 サービス資格情報の管理方法と管理場所は、お客様ごとに一意です。

簡単にするため、このチュートリアルでは、コマンドラインからサービス資格情報を渡します。 ただし、IT セキュリティチームと協力して、組織のセキュリティガイドラインに従ってこれらの資格情報を保存し、アクセスする方法を理解してください。

1. を [「サービス資格情報」JSON をダウンロードしました。](#download-service-credentials) 次の名前のファイルに `service_token.json` プロジェクトのルートにある
   + ただし、Git に認証情報をコミットしないでください。

## サービス資格情報を使用

サービス資格情報（完全形式の JSON オブジェクト）は、JWT やアクセストークンとは異なります。 代わりに、（秘密鍵を含む）サービス資格情報を使用して JWT を生成し、この JWT をアクセストークン用のAdobe IMSAPI と交換します。

![サービス資格情報 — 外部アプリケーション](assets/service-credentials/service-credentials-external-application.png)

1. 安全な場所に、AEM Developer Console からサービス資格情報をダウンロードします。
1. 外部アプリケーションは、AEMas a Cloud Service環境とプログラム的にやり取りする必要があります
1. 外部アプリケーションは、安全な場所からサービス資格情報を読み取ります
1. 外部アプリケーションは、サービス資格情報からの情報を使用して JWT トークンを構築します
1. JWT トークンは、アクセストークンと交換するためにAdobe IMSに送信されます。
1. Adobe IMSは、AEM as a Cloud Serviceへのアクセスに使用できるアクセストークンを返します
   + アクセストークンに有効期限をリクエストできます。 アクセストークンの有効期間を短くし、必要に応じて更新することをお勧めします。
1. 外部アプリケーションは、AEMに HTTP リクエストをas a Cloud Serviceし、アクセストークンを Bearer トークンとして HTTP リクエストの Authorization ヘッダーに追加します
1. AEM as a Cloud Serviceは、HTTP リクエストを受信し、リクエストを認証し、HTTP リクエストによってリクエストされた作業を実行し、HTTP レスポンスを外部アプリケーションに返します

### 外部アプリケーションの更新

サービス資格情報を使用してas a Cloud ServiceのAEMにアクセスするには、外部アプリケーションを次の 3 つの方法で更新する必要があります。

1. サービス資格情報を読み取る

+ 簡単にするために、アドビはこれらをダウンロードした JSON ファイルから読み取りますが、実際のシナリオでは、サービス資格情報は組織のセキュリティガイドラインに従って安全に保存する必要があります

1. サービス資格情報から JWT を生成
1. JWT をアクセストークンと交換する

+ サービス資格情報が存在する場合、外部アプリケーションは、AEM as a Cloud Serviceにアクセスする際に、ローカル開発アクセストークンではなく、このアクセストークンを使用します

このチュートリアルでは、Adobeの `@adobe/jwt-auth` npm モジュールは、両方に使用されます (1)。(1) サービス資格情報から JWT を生成し、(2)1 回の関数呼び出しでアクセストークンと交換します。 アプリケーションが JavaScript ベースではない場合は、 [他の言語のサンプルコード](https://developer.adobe.com/developer-console/docs/guides/) を参照してください。

## サービス資格情報の読み取り

以下を確認します。 `getCommandLineParams()` 「サービスアクセストークン JSON 」で読み取るのと同じコードを使用して、サービス資格情報 JSON ファイルを読み取れることを確認します。

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

## JWT の作成とアクセストークンの交換

サービス資格情報が読み取られると、それらを使用して JWT が生成され、その JWT がアクセストークン用のAdobe IMSAPI と交換されます。 その後、このアクセストークンを使用して、AEM as a Cloud Serviceにアクセスできます。

このサンプルアプリケーションは Node.js ベースなので、 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm モジュール： (1) JWT の生成と (20 のAdobe IMS交換を容易にする )。 アプリケーションが別の言語で開発されている場合は、 [適切なコードサンプル](https://developer.adobe.com/developer-console/docs/guides/) 他のプログラミング言語を使用してAdobe IMSに対する HTTP リクエストを作成する方法に関する

1. を更新します。 `getAccessToken(..)` をクリックして JSON ファイルの内容を調べ、それがローカル開発のアクセストークンかサービス資格情報かを判断します。 これは、 `.accessToken` プロパティに含まれます。このプロパティはローカル開発のアクセストークン JSON に対してのみ存在します。

   サービス資格情報が指定されている場合、アプリケーションは JWT を生成し、アクセストークンのAdobe IMSと交換します。 ここでは、 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)&#39;s `auth(...)` 関数を使用します。この関数は、JWT を生成し、1 回の関数呼び出しでアクセストークンと交換します。 パラメーター `auth(..)` メソッドは [特定の情報で構成される JSON オブジェクト](https://www.npmjs.com/package/@adobe/jwt-auth#config-object) は、以下のコードで説明されているように、サービス資格情報 JSON から使用できます。

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

    現在は、ローカル開発のアクセストークン JSON またはサービス資格情報 JSON のどちらかの JSON ファイルが、「file」コマンドラインパラメーターを介して渡されるかに応じて、アプリケーションはアクセストークンを取得します。
    
    サービス資格情報は 365 日ごとに期限切れになりますが、JWT と対応するアクセストークンは頻繁に期限切れになり、期限切れになる前に更新する必要があります。 これは、[Adobe IMSが提供する ](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens) を使用して実行できます。

1. これらの変更がおこなわれたので、サービス資格情報 JSON はAEM Developer Console からダウンロードされ、簡単にするために、次のように保存されました。 `service_token.json` これと同じフォルダー内 `index.js`. 次に、コマンドラインパラメーターを置き換えてアプリケーションを実行します。 `file` と `service_token.json`、 `propertyValue` を新しい値に変更すると、効果がAEMに表示されます。

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

   この __403 — 禁止__ 行には、AEM as a Cloud Serviceへの HTTP API 呼び出しでエラーが示されます。 これらの 403 Forbidden エラーは、アセットのメタデータを更新しようとしたときに発生します。

   これは、サービス資格情報派生アクセストークンが、自動作成されたテクニカルアカウントAEMユーザーを使用して、AEMへのリクエストを認証するためです。デフォルトでは、読み取りアクセスのみが許可されています。 AEMへのアプリケーション書き込みアクセス権を付与するには、アクセストークンに関連付けられているテクニカルアカウントAEMユーザーにAEMでの権限が付与されている必要があります。

## AEMでのアクセスの設定

サービス資格情報派生アクセストークンは、Contributors AEMユーザーグループのメンバーシップを持つテクニカルアカウントAEM User を使用します。

![サービス資格情報 — テクニカルアカウントAEMユーザー](./assets/service-credentials/technical-account-user.png)

テクニカルアカウントAEMユーザーがAEMに（アクセストークンを使用した最初の HTTP リクエストの後に）存在したら、このAEMユーザーの権限は他のAEMユーザーと同じように管理できます。

1. まず、AEM Developer Console からダウンロードしたサービス資格情報 JSON を開き、テクニカルアカウントのAEMログイン名を探し、 `integration.email` の値は次のようになります。 `12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. 対応するAEM環境のオーサーサービスにAEM管理者としてログインします。
1. に移動します。 __ツール__ > __セキュリティ__ > __ユーザー__
1. AEMユーザーを見つけ、 __ログイン名__ 手順 1 で特定し、 __プロパティ__
1. 次に移動： __グループ__ タブをクリックし、 __DAM ユーザー__ グループ（アセットへの書き込みアクセス権としてのグループ）
1. タップ __保存して閉じる__

AEMでアセットに対する書き込み権限を持つテクニカルアカウントが許可された状態で、アプリケーションを再実行します。

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

## 変更の検証

1. 更新されたAEMas a Cloud Service環境にログインします ( `aem` コマンドラインパラメータ )
1. 次に移動： __Assets__ > __ファイル__
1. これにより、 `folder` コマンドラインパラメータ（例： ） __WKND__ > __英語__ > __冒険__ > __ナパワインの試飲会__
1. を開きます。 __プロパティ__ （フォルダー内の任意のアセット）
1. 次に移動： __詳細__ タブ
1. 更新されたプロパティの値を確認します（例： ）。 __著作権__ 更新された `metadata/dc:rights` JCR プロパティ。 `propertyValue` パラメーター、例： __WKND 制限使用__

![WKND 制限付きメタデータ更新の使用](./assets/service-credentials/asset-metadata.png)

## おめでとうございます。

これで、ローカル開発のアクセストークンと実稼動準備のサービス間アクセストークンを使用して、AEMas a Cloud Serviceにプログラムでアクセスできるようになりました。
