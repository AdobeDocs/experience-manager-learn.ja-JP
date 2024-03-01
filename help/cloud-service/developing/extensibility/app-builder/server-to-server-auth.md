---
title: App Builder アクションでのサーバー間アクセストークンの生成
description: App Builder アクションで使用する OAuth サーバー間資格情報を使用して、アクセストークンを生成する方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-14724
last-substantial-update: 2024-02-29T00:00:00Z
duration: null
source-git-commit: 8fae7510db1eb7b9483d198592a1628cd9867e2b
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 8%

---

# App Builder アクションでのサーバー間アクセストークンの生成

App Builder のアクションは、をサポートするAdobeAPI とやり取りする必要が出る場合があります **OAuth サーバー間資格情報** とは、App Builder アプリがデプロイされるAdobe Developer Console プロジェクトに関連付けられています。

このガイドでは、 _OAuth サーバー間資格情報_ App Builder アクションで使用する場合。

>[!IMPORTANT]
>
> OAuth サーバー間資格情報は、サービスアカウント (JWT) 資格情報で非推奨（廃止予定）になりました。 ただし、サービスアカウント (JWT) 資格情報のみをサポートするAdobeAPI や、OAuth サーバー間の移行が進行中の場合もあります。 AdobeAPI のドキュメントを確認して、サポートされる資格情報を理解します。

## Adobe Developer Console プロジェクト設定

目的のAdobeAPI をAdobe Developer Console プロジェクトに追加する際、 _API の設定_ ステップ、 **OAuth サーバー間通信** 認証タイプ。

![Adobe Developerコンソール — OAuth サーバー間](./assets/s2s-auth/oauth-server-to-server.png)

上記の自動作成された統合サービスアカウントを割り当てるには、目的の製品プロファイルを選択します。 したがって、製品プロファイルを介して、サービスアカウントの権限が制御されます。

![Adobe Developer Console — 製品プロファイル](./assets/s2s-auth/select-product-profile.png)

## .env ファイル

App Builder プロジェクトの `.env` ファイルに、Adobe Developer Console プロジェクトの OAuth Server-to-Server 資格情報用のカスタムキーを追加します。 OAuth サーバー間秘密鍵証明書の値は、Adobe Developer Console プロジェクトの __資格情報__ > __OAuth サーバー間通信__ 特定のワークスペースの

![Adobe Developerコンソール OAuth サーバー間資格情報](./assets/s2s-auth/oauth-server-to-server-credentials.png)

```
...
OAUTHS2S_CLIENT_ID=58b23182d80a40fea8b12bc236d71167
OAUTHS2S_CLIENT_SECRET=p8e-EIRF6kY6EHLBSdw2b-pLUWKodDqJqSz3
OAUTHS2S_CECREDENTIALS_METASCOPES=AdobeID,openid,ab.manage,additional_info.projectedProductContext,read_organizations,read_profile,account_cluster.read
```

値： `OAUTHS2S_CLIENT_ID`, `OAUTHS2S_CLIENT_SECRET`, `OAUTHS2S_CECREDENTIALS_METASCOPES` は、Adobe Developer Console プロジェクトの OAuth サーバー間資格情報画面から直接コピーできます。

## 入力マッピング

OAuth Server-to-Server 秘密鍵証明書の値を `.env` ファイルにマッピングする場合は、AppBuilder のアクション入力にマッピングし、アクション自体で読み取れるようにする必要があります。 これを行うには、`ext.config.yaml` アクション `inputs` の各変数のエントリを `PARAMS_INPUT_NAME: $ENV_KEY` の形式で追加します。

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

App Builder のアクションでは、OAuth サーバー間資格情報は、 `params` オブジェクト。 これらの資格情報を使用すると、 [OAuth 2.0 ライブラリ](https://oauth.net/code/). または、 [ノード取得ライブラリ](https://www.npmjs.com/package/node-fetch) をクリックして、POSTトークンエンドポイントにAdobe IMSリクエストを送信し、アクセストークンを取得します。

次の例は、 `node-fetch` ライブラリを使用して、POSTトークンエンドポイントにAdobe IMSリクエストを実行し、アクセストークンを取得します。

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
