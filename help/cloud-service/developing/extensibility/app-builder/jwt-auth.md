---
title: App Builder アクションでの JWT アクセストークンの生成
description: App Builder アクション用の JWT 資格情報を使用してアクセストークンを生成する方法について説明します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-11743
last-substantial-update: 2023-01-17T00:00:00Z
exl-id: 9a3fed96-c99b-43d1-9dba-a4311c65e5b9
duration: 151
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '456'
ht-degree: 100%

---

# App Builder アクションでの JWT アクセストークンの生成

App Builder アクションは、App Builder アプリがデプロイされる Adobe Developer Console プロジェクトに関連付けられた Adobe API とやり取りする必要が生じる場合があります。

場合により、これには、目的とする Adobe Developer Console プロジェクトに関連付けられた独自の JWT アクセストークンを App Builder アクションで生成する必要があります。

>[!IMPORTANT]
>
> [App Builder セキュリティドキュメント](https://developer.adobe.com/app-builder/docs/guides/security/)を参照して、アクセストークンを生成するのが適切な場合と、提供されたアクセストークンを使用するのが適切な場合を確認してください。
>
> カスタムアクションでは、許可されたコンシューマーのみが App Builder アクションとその背後にあるアドビのサービスにアクセスできるように、独自のセキュリティチェックを提供する必要が生じる場合があります。


## .env ファイル

App Builder プロジェクトの `.env` ファイルに、Adobe Developer Console プロジェクトの各 JWT 資格情報のカスタムキーを追加します。JWT 資格情報の値は、Adobe Developer Console プロジェクトの&#x200B;__資格情報__／__サービスアカウント（JWT）__ から指定のワークスペースに対して取得できます。

![Adobe Developer Console JWT サービスの資格情報](./assets/jwt-auth/jwt-credentials.png)

```
...
JWT_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
JWT_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
JWT_TECHNICAL_ACCOUNT_ID=1F072B8A63C6E0230A495EE1@techacct.adobe.com
JWT_IMS_ORG=7ABB3E6A5A7491460A495D61@AdobeOrg
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

`JWT_CLIENT_ID`、`JWT_CLIENT_SECRET`、`JWT_TECHNICAL_ACCOUNT_ID`、`JWT_IMS_ORG` の値は、Adobe Developer Console プロジェクトの JWT 資格情報画面から直接コピーできます。

### メタスコープ

App Builder アクションがやりとりする Adobe API とそのメタスコープを決定します。`JWT_METASCOPES` キーにコンマ区切り記号を使用してメタスコープを一覧表示します。有効なメタコードは、[アドビの JWT メタスコープドキュメント](https://developer.adobe.com/developer-console/docs/guides/authentication/JWT/Scopes/)にリストアップされています。


例えば、次の値が `.env` の `JWT_METASCOPES` キーに追加される場合があります。

```
...
JWT_METASCOPES=https://ims-na1.adobelogin.com/s/ent_analytics_bulk_ingest_sdk,https://ims-na1.adobelogin.com/s/event_receiver_api
...
```

### 秘密鍵

`JWT_PRIVATE_KEY` はもともと複数行の値であり、`.env` ファイルではサポートされていないため、特別にフォーマットする必要があります。最も簡単な方法は、プライベートキーを base64 でエンコードすることです。秘密鍵（`-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----`）の Base64 エンコードは、オペレーティングシステムが提供するネイティブツールを使用して実行できます。

>[!BEGINTABS]

>[!TAB macOS]

1. `Terminal` を開きます
1. `base64 -i /path/to/private.key | pbcopy` コマンドを実行します
1. base64 出力は、クリップボードに自動的にコピーされます
1. 対応するキーの値として `.env` に貼り付けます

>[!TAB Windows]

1. `Command Prompt` を開きます
1. `certutil -encode C:\path\to\private.key C:\path\to\encoded-private.key` コマンドを実行します
1. `findstr /v CERTIFICATE C:\path\to\encoded-private.key` コマンドを実行します
1. base64 出力をクリップボードにコピーします
1. 対応するキーの値として `.env` に貼り付けます

>[!TAB Linux®]

1. ターミナルを開きます
1. `base64 private.key` コマンドを実行します
1. base64 出力をクリップボードにコピーします
1. 対応するキーの値として `.env` に貼り付けます

>[!ENDTABS]

例えば、次の base64 でエンコードされた秘密鍵が `.env` の `JWT_PRIVATE_KEY` キーに追加される場合があります。

```
...
JWT_PRIVATE_KEY=LS0tLS1C..kQgUFJJVkFURSBLRVktLS0tLQ==
```

## 入力マッピング

`.env` ファイルにある JWT 資格情報の値セットを使用して、アクション自体で読み取ることができるように、それらを AppBuilder アクションの入力にマッピングする必要があります。これを行うには、`ext.config.yaml` アクション `inputs` の各変数のエントリを `PARAMS_INPUT_NAME: $ENV_KEY` の形式で追加します。

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

`inputs` で定義されたキーは、App Builder アクションに提供される `params` オブジェクトで使用できます。


## トークンにアクセスするための JWT 資格情報

App Builder アクションでは、JWT 資格情報は `params` オブジェクトで使用でき、[`@adobe/jwt-auth`](https://www.npmjs.com/package/@adobe/jwt-auth) でアクセストークンを生成するために使用できます。これにより、他の Adobe API およびサービスにアクセスできます。

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
