---
title: App Builder アクションでのアクセストークンの生成
description: App Builder アクションで使用する JWT 資格情報を使用してアクセストークンを生成する方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
kt: 11743
last-substantial-update: 2023-01-17T00:00:00Z
source-git-commit: 40679e80fd9270dd9fad8174a986fd1fdd5e3d29
workflow-type: tm+mt
source-wordcount: '469'
ht-degree: 1%

---


# App Builder アクションでのアクセストークンの生成

App Builder アクションは、App Builder アプリがデプロイされるAdobe Developerコンソールプロジェクトに関連付けられたAdobeAPI とやり取りする必要が生じる場合があります。

この場合、目的のAdobe Developer Console プロジェクトに関連付けられた独自のアクセストークンを生成するために、App Builder アクションが必要になることがあります。

>[!IMPORTANT]
>
> レビュー [App Builder のセキュリティドキュメント](https://developer.adobe.com/app-builder/docs/guides/security/) を参照して、提供されたアクセストークンを使用するのに対して、アクセストークンを生成する必要があるタイミングを把握します。
>
> カスタムアクションでは、許可されたコンシューマーのみが App Builder アクションとその背後にあるAdobe サービスにアクセスできるように、独自のセキュリティチェックを提供する必要が生じる場合があります。


## .env ファイル

App Builder プロジェクトの `.env` ファイルに、Adobe Developer Console プロジェクトの JWT 資格情報ごとにカスタムキーを追加します。 JWT 秘密鍵証明書の値は、Adobe Developer Console プロジェクトの __資格情報__ > __サービスアカウント (JWT)__ 特定のワークスペースの

![Adobe Developer Console JWT サービス資格情報](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

次の値： `JWT_CLIENT_ID`, `JWT_CLIENT_SECRET`, `JWT_TECHNICAL_ACCOUNT_ID`, `JWT_IMS_ORG` は、Adobe Developer Console プロジェクトの JWT 資格情報画面から直接コピーできます。

### メタスコープ

App Builder のAdobeAPI とそのメタコピーを特定し、App Builder のアクションがやり取りする。 コンマ区切り文字を使用したメタコードのリスト `JWT_METASCOPES` キー。 有効なメタコードは、 [Adobeの JWT メタスコープドキュメント](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/).


例えば、次の値が `JWT_METASCOPES` キー `.env`:

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 秘密鍵

この `JWT_PRIVATE_KEY` は、ネイティブで複数行の値（ではサポートされていない）なので、特別な形式にする必要があります。 `.env` ファイル。 最も簡単な方法は、秘密鍵を base64 でエンコードすることです。 秘密鍵の Base64 エンコード (`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`) は、オペレーティングシステムで提供されるネイティブツールを使用しておこなうことができます。

>[!BEGINTABS]

>[!TAB macOS]

1. `Terminal` を開きます。
1. コマンドを実行 `base64 -i /path/to/private.key | pbcopy`
1. base64 出力は、クリップボードに自動的にコピーされます
1. 貼り付け先 `.env` 対応するキーの値

>[!TAB Windows]


1. `Command Prompt` を開きます。
1. コマンドを実行 `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key`
1. コマンドを実行 `findstr /v CERTIFICATE C:\path\to\encoded-private.key`
1. base64 出力をクリップボードにコピーします。
1. 貼り付け先 `.env` 対応するキーの値

>[!TAB Linux®]

1. ターミナルを開く
1. コマンドを実行 `base64 private.key`
1. base64 出力をクリップボードにコピーします。
1. 貼り付け先 `.env` 対応するキーの値

>[!ENDTABS]

例えば、次の base64 でエンコードされた秘密鍵が `JWT_PRIVATE_KEY` キー `.env`:

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 入力マッピング

JWT 秘密鍵証明書の値を `.env` ファイルにマッピングする場合は、AppBuilder のアクション入力にマッピングし、アクション自体で読み取れるようにする必要があります。 これをおこなうには、 `ext.config.yaml` アクション `inputs` を次の形式で指定します。 `PARAMS_INPUT_NAME: $ENV_KEY`.

次に例を示します。

```yaml
operations:
  view:
    - type: web
      impl: index.html
actions: actions
runtimeManifest:
  packages:
    dx-excshell-1:
      license: Apache-2.0
      actions:
        generic:
          function: actions/generic/index.js
          web: 'yes'
          runtime: nodejs:16
          inputs:
            LOG_LEVEL: debug
            JWT_CLIENT_ID: $JWT_CLIENT_ID
            JWT_TECHNICAL_ACCOUNT_ID: $JWT_TECHNICAL_ACCOUNT_ID
            JWT_IMS_ORG: $JWT_IMS_ORG
            JWT_METASCOPES: $JWT_METASCOPES
            JWT_PRIVATE_KEY: $JWT_PRIVATE_KEY
          annotations:
            require-adobe-auth: false
            final: true
```

以下で定義されたキー `inputs` は `params` オブジェクトが App Builder アクションに提供されたとき。


## トークンにアクセスするための JWT 資格情報

「App Builder」アクションでは、JWT 資格情報が `params` オブジェクトで、 [`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) を使用してアクセストークンを生成し、他のAdobeAPI やサービスにアクセスできる

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");
const auth = require("@adobe/jwt-auth");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, [
            "JWT_CLIENT_ID", "JWT_TECHNICAL_ACCOUNT_ID", "JWT_IMS_ORG", "JWT_CLIENT_SECRET", "JWT_METASCOPES", "JWT_PRIVATE_KEY"], []);

    // Split the metascopes into an array (they are comma delimited in the .env file)
    const metascopes = params.JWT_METASCOPES?.split(',') || [];

    // Base64 decode the private key value
    const privateKey = Buffer.from(params.JWT_PRIVATE_KEY, 'base64').toString('utf-8');

    // Exchange the JWT credentials for an 24-hour Access Token
    let { accessToken } = await auth({
      clientId: params.JWT_CLIENT_ID,                          // Client Id
      technicalAccountId: params.JWT_TECHNICAL_ACCOUNT_ID,     // Technical Account Id
      orgId: params.JWT_IMS_ORG,                               // Adobe IMS Org Id
      clientSecret: params.JWT_CLIENT_SECRET,                  // Client Secret
      metaScopes: metascopes,                                  // Metadcopes defining level of access the access token should provide
      privateKey: privateKey,                                  // Private Key to sign the JWT
    });

    // The 24-hour IMS Access Token is used to call the Analytics APIs
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = await getAccessToken(params);

    // Invoke an exmaple Adobe API endpoint using the generated accessToken
    const res = await fetch('https://analytics.adobe.io/api/example/reports', {
      headers: {
        "Accept": "application/json",
        "Content-Type": "application/json",
        "X-Proxy-Global-Company-Id": 'example',
        "Authorization": `Bearer ${accessToken}`,
        "x-Api-Key": params.JWT_CLIENT_ID,
      },
      method: "POST",
      body: JSON.stringify({... An Analytics query ... }),
    });

    if (!res.ok) { throw new Error("Request to API failed with status code " + res.status);}

    // Analytics API data
    let data = await res.json();

    const response = {
      statusCode: 200,
      body: data,
    };

    return response;
  } catch (error) {
    logger.error(error);
    return errorResponse(500, "server error", logger);
  }
}

exports.main = main;
```
