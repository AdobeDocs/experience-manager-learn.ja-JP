---
title: App Builder アクションでのサーバー間アクセストークンの生成
description: App Builder アクションで使用する OAuth サーバー間認証情報を使用して、アクセストークンを生成する方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: 122
exl-id: 919cb9de-68f8-4380-940a-17274183298f
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '400'
ht-degree: 100%

---

# App Builder アクションでのサーバー間アクセストークンの生成

App Builder アクションは、**OAuth サーバー間認証情報**&#x200B;をサポートし、App Builder アプリがデプロイされる Adobe Developer Console プロジェクトに関連付けられた Adobe API とやり取りする必要が生じる場合があります。

このガイドでは、App Builder アクションで使用する _OAuth サーバー間認証情報_&#x200B;を使用して、アクセストークンを生成する方法について説明します。

>[!IMPORTANT]
>
> サービスアカウント（JWT）資格情報は、OAuth サーバー間認証情報に代わって非推奨（廃止予定）になりました。ただし、サービスアカウント（JWT）資格情報のみをサポートする Adobe API や、OAuth サーバー間の移行が進行中の場合もあります。Adobe API のドキュメントを確認して、サポートされている資格情報を理解します。

## Adobe Developer Console プロジェクトの設定

目的の Adobe API を Adobe Developer Console プロジェクトに追加する際、_API の設定_&#x200B;手順で、**OAuth サーバー間**&#x200B;の認証タイプを選択します。

![Adobe Developer Console - OAuth サーバー間](./assets/s2s-auth/oauth-server-to-server.png)

上記の自動作成された統合サービスアカウントを割り当てるには、目的の製品プロファイルを選択します。したがって、製品プロファイルを介して、サービスアカウントの権限が制御されます。

![Adobe Developer Console - 製品プロファイル](./assets/s2s-auth/select-product-profile.png)

## .env ファイル

App Builder プロジェクトの `.env` ファイルに、Adobe Developer Console プロジェクトの OAuth サーバー間資格情報のカスタムキーを追加します。OAuth サーバー間資格情報の値は、Adobe Developer Console プロジェクトの&#x200B;__資格情報__／__OAuth サーバー間__&#x200B;から指定のワークスペースに対して取得できます。

![Adobe Developer Console OAuth サーバー間の資格情報](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

`OAUTHS2S_CLIENT_ID`、`OAUTHS2S_CLIENT_SECRET`、`OAUTHS2S_CECREDENTIALS_METASCOPES` の値は、Adobe Developer Console プロジェクトの OAuth サーバー間資格情報画面から直接コピーできます。

## 入力マッピング

`.env` ファイルにある OAuth サーバー間資格情報の値セットを使用して、アクション自体で読み取ることができるように、それらを AppBuilder アクションの入力にマッピングする必要があります。これを行うには、`ext.config.yaml` アクション `inputs` の各変数のエントリを `PARAMS_INPUT_NAME: $ENV_KEY` の形式で追加します。

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
            OAUTHS2S_CLIENT_ID: $OAUTHS2S_CLIENT_ID
            OAUTHS2S_CLIENT_SECRET: $OAUTHS2S_CLIENT_SECRET
            OAUTHS2S_CECREDENTIALS_METASCOPES: $OAUTHS2S_CECREDENTIALS_METASCOPES
          annotations:
            require-adobe-auth: false
            final: true
```

`inputs` で定義されたキーは、App Builder アクションに提供される `params` オブジェクトで使用できます。

## トークンにアクセスするための OAuth サーバー間資格情報

App Builder のアクションでは、OAuth サーバー間資格情報は `params` オブジェクトで使用できます。これらの資格情報を使用すると、[OAuth 2.0 ライブラリ](https://oauth.net/code/)を使用してアクセストークンを生成できます。または、[ノード取得ライブラリ](https://www.npmjs.com/package/node-fetch)を使用して、Adobe IMS トークンエンドポイントに POST リクエストを送信して、アクセストークンを取得します。

次の例は、`node-fetch` ライブラリを使用して、Adobe IMS トークンエンドポイントに POST リクエストを実行し、アクセストークンを取得する方法を示しています。

```javascript
const fetch = require("node-fetch");
const { Core } = require("@adobe/aio-sdk");
const { errorResponse, stringParameters, checkMissingRequestInputs } = require("../utils");

async function main(params) {
  const logger = Core.Logger("main", { level: params.LOG_LEVEL || "info" });

  try {
    // Perform any necessary input error checking
    const systemErrorMessage = checkMissingRequestInputs(params, ["OAUTHS2S_CLIENT_ID", "OAUTHS2S_CLIENT_SECRET", "OAUTHS2S_CECREDENTIALS_METASCOPES"], []);

    // The Adobe IMS token endpoint URL
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';

    // The POST request options
    const options = {
        method: 'POST',
        headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        },
        body: `grant_type=client_credentials&client_id=${params.OAUTHS2S_CLIENT_ID}&client_secret=${params.OAUTHS2S_CLIENT_SECRET}&scope=${params.OAUTHS2S_CECREDENTIALS_METASCOPES}`,
    };

    // Make a POST request to the Adobe IMS token endpoint to get the access token
    const tokenResponse = await fetch(adobeIMSV3TokenEndpointURL, options);
    const tokenResponseJSON = await tokenResponse.json();

    // The 24-hour IMS Access Token is used to call the AEM Data Service API
    // Can look at caching this token for 24 hours to reduce calls
    const accessToken = tokenResponseJSON.access_token;

    // Invoke an AEM Data Service API using the access token
    const aemDataResponse = await fetch(`https://api.adobeaemcloud.com/adobe/stats/statistics/contentRequestsQuota?imsOrgId=${IMS_ORG_ID}&current=true`, {
      headers: {
        'X-Adobe-Accept-Experimental': '1',
        'x-gw-ims-org-id': IMS_ORG_ID,
        'X-Api-Key': params.OAUTHS2S_CLIENT_ID,
        Authorization: `Bearer ${access_token}`, // The 24-hour IMS Access Token
      },
      method: "GET",
    });

    if (!aemDataResponse.ok) { throw new Error("Request to API failed with status code " + aemDataResponse.status);}

    // API data
    let data = await aemDataResponse.json();

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
