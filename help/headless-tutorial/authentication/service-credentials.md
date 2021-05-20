---
title: サービス資格情報
description: AEMサービス資格情報は、HTTPを介してAEMオーサーサービスまたはパブリッシュサービスとプログラム的にやり取りする外部のアプリケーション、システム、サービスを容易にするために使用されます。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330519.jpg
topic: ヘッドレス、統合
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1827'
ht-degree: 0%

---


# サービス資格情報

AEM as a AEMとの統合では、Cloud Serviceに対して安全に認証できる必要があります。 AEM Developer Consoleは、外部のアプリケーション、システムおよびサービスがHTTP経由でAEMオーサーまたはパブリッシュサービスとプログラム的にやり取りするのを容易にするために使用されるサービス資格情報へのアクセスを許可します。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

サービス資格情報は、[ローカル開発アクセストークン](./local-development-access-token.md)と似ている場合がありますが、いくつかの主な方法で異なります。

+ サービス資格情報は、_アクセストークンではなく_、__&#x200B;アクセストークンの取得に使用される資格情報です。
+ サービス資格情報はより永続的で（365日ごとに期限切れになります）、ローカル開発アクセストークンは毎日期限切れになるのに対して、失効しない限り変更されません。
+ AEM as a AEMCloud Service環境のサービス資格情報は、単一のテクニカルアカウントユーザーにマッピングされます。一方、ローカル開発アクセストークンは、アクセストークンを生成したAEMユーザーとして認証されます。

3つすべてをCloud Service環境として各AEMへのアクセス権を取得するために使用できるので、サービス資格情報と、生成するアクセストークンの両方を秘密にする必要があります

## サービス資格情報の生成

サービス資格情報の生成は、次の2つの手順に分かれます。

1. AdobeIMS Org管理者による1回限りのサービス資格情報の初期化
1. サービス資格情報JSONのダウンロードと使用

### サービス資格情報の初期化

ローカル開発アクセストークンとは異なり、サービス資格情報をダウンロードするには、AdobeOrg IMS管理者が&#x200B;_1回限りの初期化_&#x200B;をおこなう必要があります。

![サービス資格情報の初期化](assets/service-credentials/initialize-service-credentials.png)

__これは、AEM as a Cloud Service環境ごとに1回のみ初期化されます。__

1. AdobeIMS Orgの管理者としてログインしていることを確認します。
1. [AdobeCloud Manager](https://my.cloudmanager.adobe.com)にログインします。
1. AEM as a Cloud Service環境を含むプログラムを開き、のサービス資格情報を設定できるようにします。
1. 「__環境__」セクションで環境の横にある省略記号をタップし、「__開発者コンソール__」を選択します。
1. 「__統合__」タブをタップします。
1. 「__サービス資格情報を取得__」ボタンをタップします。
1. サービス資格情報が初期化され、JSONとして表示されます

![AEM開発者コンソール — 統合 — サービス資格情報の取得](./assets/service-credentials/developer-console.png)

AEM as a Cloud Service環境のサービス資格情報が初期化されると、AdobeIMS組織の他のAEM開発者がそれらをダウンロードできます。

### サービス資格情報のダウンロード

![サービス資格情報のダウンロード](assets/service-credentials/download-service-credentials.png)

サービス資格情報のダウンロードは、初期化と同じ手順に従います。 初期化がまだおこなわれていない場合は、「__サービス資格情報を取得__」ボタンをタップすると、エラーが表示されます。

1. 自分が&#x200B;__Cloud Manager - Developer__ IMS製品プロファイル(AEM Developer Consoleへのアクセスを許可)のメンバーであることを確認します。
   + Cloud Service環境としてのSandbox AEMは、__AEM Administrators__&#x200B;または&#x200B;__AEM Users__&#x200B;製品プロファイルのメンバーシップのみ必要です
1. [AdobeCloud Manager](https://my.cloudmanager.adobe.com)にログインします。
1. と統合するAEM環境としてのCloud Serviceを含むプログラムを開きます。
1. 「__環境__」セクションで環境の横にある省略記号をタップし、「__開発者コンソール__」を選択します。
1. 「__統合__」タブをタップします。
1. 「__サービス資格情報を取得__」ボタンをタップします。
1. 左上隅のダウンロードボタンをタップして、サービス資格情報の値が含まれているJSONファイルをダウンロードし、安全な場所に保存します。
   + _サービス資格情報に問題が発生した場合は、すぐにAdobeサポートに連絡して、失効させてもらってください_

## サービス資格情報のインストール

サービス資格情報は、JWTの生成に必要な詳細を提供します。JWTは、AEM as a Cloud Serviceでの認証に使用されるアクセストークンと交換されます。 サービス資格情報は、AEMにアクセスするために使用する外部アプリケーション、システム、またはサービスがアクセスできる安全な場所に保存する必要があります。 サービス資格情報の管理方法と管理場所は、お客様ごとに一意です。

簡単にするために、このチュートリアルでは、にサービス資格情報をコマンドラインで渡しますが、ITセキュリティチームと協力して、組織のセキュリティガイドラインに従ってこれらの資格情報を保存し、アクセスする方法を理解します。

1. ダウンロードした[サービス資格情報JSON](#download-service-credentials)を、プロジェクトのルートにある`service_token.json`という名前のファイルにコピーします。
   + しかし、Gitに資格情報をコミットしないでください。

## サービス資格情報を使用

完全形式のJSONオブジェクトであるサービス資格情報は、JWTやアクセストークンとは異なります。 代わりに、（秘密鍵を含む）サービス資格情報を使用してJWTを生成し、JWTを使用してアクセストークン用のAdobeIMS APIと交換します。

![サービス資格情報 — 外部アプリケーション](assets/service-credentials/service-credentials-external-application.png)

1. サービス資格情報をAEM Developer Consoleから安全な場所にダウンロードします。
1. 外部アプリケーションは、プログラム環境としてAEMとやり取りする必要があります。Cloud Service
1. 外部アプリケーションは、安全な場所からサービス資格情報を読み込みます
1. 外部アプリケーションは、サービス資格情報の情報を使用してJWTトークンを構築します
1. JWTトークンがAdobeIMSに送信され、アクセストークンと交換されます。
1. AdobeIMSは、AEMにCloud Serviceとしてアクセスするために使用できるアクセストークンを返します
   + アクセストークンには、有効期限をリクエストできます。 アクセストークンの有効期間を短くし、必要に応じて更新することをお勧めします。
1. 外部アプリケーションは、HTTPリクエストをCloud ServiceとしてAEMに対しておこない、アクセストークンをBearerトークンとしてHTTPリクエストのAuthorizationヘッダーに追加します
1. AEM as aCloud ServiceはHTTPリクエストを受信し、リクエストを認証し、HTTPリクエストによってリクエストされた作業を実行し、HTTPレスポンスを外部アプリケーションに返します

### 外部アプリケーションの更新

サービス資格情報を使用してAEM as aCloud Serviceにアクセスするには、外部アプリケーションを次の3つの方法で更新する必要があります。

1. サービス資格情報を読み取る
   + 簡単にするために、ダウンロードしたJSONファイルからこれらを読み取りますが、実際に使用するシナリオでは、サービス資格情報は組織のセキュリティガイドラインに従って安全に保存する必要があります
1. サービス資格情報からJWTを生成
1. JWTをアクセストークンと交換する
   + サービス資格情報が存在する場合、外部アプリケーションは、AEMにCloud Serviceとしてアクセスする際に、ローカル開発アクセストークンの代わりにこのアクセストークンを使用します

このチュートリアルでは、Adobeの`@adobe/jwt-auth` npmモジュールを使用して、(1)サービス資格情報からJWTを生成し、(2)1回の関数呼び出しでアクセストークンと交換します。 アプリケーションがJavaScriptベースでない場合は、他の言語の[サンプルコード](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)で、サービス資格情報からJWTを作成し、AdobeIMSでアクセストークンと交換する方法を確認してください。

## サービス資格情報の読み取り

`getCommandLineParams()`を確認し、ローカル開発アクセストークンJSONで読み取るのと同じコードを使用して、サービス資格情報JSONファイルを読み取れることを確認します。

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

## JWTの作成とアクセストークンの交換

サービス資格情報を読み取ると、JWTを生成するために使用され、JWTがアクセストークン用にAdobeIMS APIと交換されます。JWTはCloud ServiceとしてAEMにアクセスするために使用できます。

このサンプルAdobeはNode.jsベースなので、 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npmモジュールを使用すると、(1)JWTの生成と(20)のIMSとのやり取りが容易になります。 アプリケーションが別の言語を使用して開発されている場合は、他のプログラミング言語を使用してIMSをAdobeするHTTPリクエストの作成方法に関する[適切なコードサンプル](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/samples/samples.md)を確認してください。

1. `getAccessToken(..)`を更新して、JSONファイルの内容を調べ、それがローカル開発のアクセストークンかサービス資格情報かを判断します。 これは、ローカル開発アクセストークンJSON用にのみ存在する`.accessToken`プロパティが存在するかどうかを確認することで、簡単に実現できます。

   サービス資格情報が指定されると、アプリケーションはJWTを生成し、アクセストークンのAdobeIMSと交換します。 [@adobe/jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth)の`auth(...)`関数を使用します。この関数は、両方ともJWTを生成し、1回の関数呼び出しでアクセストークンと交換します。  `auth(..)`のパラメーターは、コードで説明するように、サービス資格情報JSONから取得できる特定の情報で構成される[JSONオブジェクトです。](https://www.npmjs.com/package/@adobe/jwt-auth#config-object)

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

   現在は、ローカル開発のアクセストークンJSONまたはサービス資格情報JSONのいずれかのJSONファイルが`file`コマンドラインパラメーターを介して渡されるかどうかに応じて、アプリケーションはアクセストークンを取得します。

   サービス資格情報には期限切れはありませんが、JWTと対応するアクセストークンには期限切れになる前に更新する必要があります。 これは、AdobeIMS](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/OAuth/OAuth.md#access-tokens)で提供される`refresh_token` [を使用しておこなうことができます。

1. これらの変更を適用し、AEM Developer Consoleからダウンロードしたサービス資格情報JSONを（簡単に、`service_token.json`と同じ`index.js`として保存するために）、コマンドラインパラメーター&lt;a2/を`service_token.json`に置き換えてアプリケーションを実行し、`propertyValue`を新しい値に更新して、効果をAEMで確認します。`file`

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Restricted Use" \
       file=service_token.json
   ```

   端末への出力は次のようになります。

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   403 - Forbidden @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

   __403 - Forbidden__&#x200B;行は、AEMをCloud Serviceとして呼び出すHTTP API呼び出しでエラーを示します。 これらの403 Forbiddenエラーは、アセットのメタデータを更新しようとしたときに発生します。

   この理由は、サービス資格情報に基づくアクセストークンが、自動作成されたテクニカルアカウントAEMユーザーを使用してAEMに対する要求を認証するためです。デフォルトでは、読み取りアクセス権のみを持ちます。 AEMへのアプリケーション書き込みアクセス権を付与するには、アクセストークンに関連付けられたテクニカルアカウントAEMユーザーにAEMでの権限を付与する必要があります。

## AEMでのアクセスの設定

サービス資格情報から派生したアクセストークンは、 Contributors AEMユーザーグループのメンバーシップを持つテクニカルアカウントAEM Userを使用します。

![サービス資格情報 — テクニカルアカウントAEM](./assets/service-credentials/technical-account-user.png)

テクニカルアカウントAEMユーザーがAEMに存在したら（最初のアクセストークンによるHTTPリクエストの後）、このAEMユーザーの権限は他のAEMユーザーと同じように管理できます。

1. まず、AEM開発者コンソールからダウンロードしたサービス資格情報JSONを開いて、テクニカルアカウントのAEMログイン名を探し、次のような`integration.email`値を探します。`12345678-abcd-9000-efgh-0987654321c@techacct.adobe.com`.
1. 対応するAEM環境のオーサーサービスにAEM管理者としてログインします。
1. __ツール__ > __セキュリティ__ > __ユーザー__&#x200B;に移動します。
1. 手順1で識別した&#x200B;__Login Name__&#x200B;を持つAEMユーザーを探し、__Properties__&#x200B;を開きます。
1. 「__グループ__」タブに移動し、__DAMユーザー__&#x200B;グループ（アセットへの書き込みアクセス権）を追加します。
1. __「保存して閉じる」__&#x200B;をタップします。

AEMでアセットに対する書き込み権限を持つ権限を持つテクニカルアカウントで、アプリケーションを再実行します。

```shell
$ node index.js \
    aem=https://author-p1234-e5678.adobeaemcloud.com \
    folder=/wknd/en/adventures/napa-wine-tasting \
    propertyName=metadata/dc:rights \
    propertyValue="WKND Restricted Use" \
    file=service_token.json
```

端末への出力は次のようになります。

```
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
```

## 変更の確認

1. （`aem`コマンドラインパラメーターで指定したのと同じホスト名を使用して）更新されたAEM as aCloud Service環境にログインします。
1. __アセット__ / __ファイル__&#x200B;に移動します。
1. `folder`コマンドラインパラメーターで指定されたアセットフォルダーに移動します（例：__WKND__ > __English__ > __Adventures__ > __Napa Wine Tasting__）。
1. フォルダー内の任意のアセットの&#x200B;__プロパティ__&#x200B;を開きます
1. 「__詳細__」タブに移動します。
1. 更新されたプロパティの値を確認します。例えば、__Copyright__&#x200B;は、更新された`metadata/dc:rights` JCRプロパティにマッピングされ、`propertyValue`パラメーターで指定された値を反映します（例：__WKND Restricted Use__）。

![WKNDでのメタデータの使用の制限の更新](./assets/service-credentials/asset-metadata.png)

## バリデーターが

これで、ローカル開発のアクセストークンと、実稼動に対応したサービス間アクセストークンを使用して、プログラムによってAEMにCloud Serviceとしてアクセスできるようになりました。

