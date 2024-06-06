---
title: Github.com webhook の検証
description: App Builder アクションでGithub.comからの Webhook リクエストを検証する方法を説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Intermediate
jira: KT-15714
last-substantial-update: 2023-06-06T00:00:00Z
source-git-commit: 4b9f784de5fff7d9ba8cf7ddbe1802c271534010
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Github.com webhook の検証

Webhook を使用すると、GitHub.comの特定のイベントを購読する統合を作成または設定できます。 これらのイベントのいずれかがトリガーされると、GitHub は HTTP POSTペイロードを Webhook の設定済み URL に送信します。 ただし、セキュリティ上の理由から、受信する Webhook リクエストが実際には悪意のあるアクターからではなく、GitHub からのものであることを確認することが重要です。 このチュートリアルでは、共有暗号鍵を使用して、Adobeの App Builder アクションでGitHub.com Webhook リクエストを検証する手順を説明します。

## AppBuilder での Github シークレットの設定

1. **秘密鍵の追加先 `.env` ファイル：**

   App Builder プロジェクトの `.env` ファイルで、GitHub.com webhook シークレットのカスタムキーを追加します。

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **更新 `ext.config.yaml` ファイル：**

   この `ext.config.yaml` GitHub.com webhook リクエストを検証するには、ファイルを更新する必要があります。

   - AppBuilder アクションの設定 `web` 設定： `raw` GitHub.comから生のリクエスト本文を受信する場合。
   - 次の下 `inputs` appbuilder アクション設定で、を追加します `GITHUB_SECRET` キー、にマッピング `.env` 秘密鍵を格納するフィールド。 このキーの値はです `.env` プレフィックスが付いたフィールド名 `$`.
   - を `require-adobe-auth` appbuilder アクション設定の注釈からへ `false` Adobe認証を必要とせずにアクションを呼び出すことを可能とする。

   結果の `ext.config.yaml` ファイルは次のようになります。

   ```yaml
   operations:
     view:
       - type: web
         impl: index.html
   actions: actions
   web: web-src
   runtimeManifest:
     packages:
       dx-excshell-1:
         license: Apache-2.0
         actions:
           github-to-jira:
             function: actions/generic/index.js
             web: 'raw'
             runtime: nodejs:20
             inputs:
               LOG_LEVEL: debug
               GITHUB_SECRET: $GITHUB_SECRET
             annotations:
               require-adobe-auth: false
               final: true
   ```

## AppBuilder アクションへの検証コードの追加

次に、以下に示す JavaScript コードを追加します（からコピーします）。 [GitHub.com のドキュメント](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example)）に設定する必要があります。 必ずを書き出してください。 `verifySignature` 関数。

```javascript
// src/dx-excshell-1/actions/generic/github-webhook-verification.js

let encoder = new TextEncoder();

async function verifySignature(secret, header, payload) {
    let parts = header.split("=");
    let sigHex = parts[1];

    let algorithm = { name: "HMAC", hash: { name: 'SHA-256' } };

    let keyBytes = encoder.encode(secret);
    let extractable = false;
    let key = await crypto.subtle.importKey(
        "raw",
        keyBytes,
        algorithm,
        extractable,
        [ "sign", "verify" ],
    );

    let sigBytes = hexToBytes(sigHex);
    let dataBytes = encoder.encode(payload);
    let equal = await crypto.subtle.verify(
        algorithm.name,
        key,
        sigBytes,
        dataBytes,
    );

    return equal;
}

function hexToBytes(hex) {
    let len = hex.length / 2;
    let bytes = new Uint8Array(len);

    let index = 0;
    for (let i = 0; i < hex.length; i += 2) {
        let c = hex.slice(i, i + 2);
        let b = parseInt(c, 16);
        bytes[index] = b;
        index += 1;
    }

    return bytes;
}

module.exports = { verifySignature };
```

## AppBuilder アクションでの検証の実装

次に、リクエストヘッダーの署名をによって生成された署名と比較して、リクエストが GitHub からのものであることを確認します。 `verifySignature` 関数。

AppBuilder アクション内 `index.js`に次のコードを追加します `main` 関数：


```javascript
// src/dx-excshell-1/actions/generic/index.js

const { verifySignature } = require("./github-webhook-verification");
...

// Main function that will be executed by Adobe I/O Runtime
async function main(params) {
  // Create a Logger
  const logger = Core.Logger("main", { level: params?.LOG_LEVEL || "info" });

  try {
    // Log parameters if LOG_LEVEL is 'debug'
    logger.debug(stringParameters(params));

    // Define required parameters and headers
    const requiredParams = [
      // Verifies the GITHUB_SECRET is present in the action's configuration; add other parameters here as needed.
      "GITHUB_SECRET"
    ];

    const requiredHeaders = [
      // Require the x-hub-signature-256 header, which GitHub.com populates with a sha256 hash of the payload
      "x-hub-signature-256"
    ];

    // Check for missing required parameters and headers
    const errorMessage = checkMissingRequestInputs(params, requiredParams, requiredHeaders);

    if (errorMessage) {
      // Return and log client errors
      return errorResponse(400, errorMessage, logger);
    }

    // Decode the request body (which is base64 encoded) to a string
    const body = Buffer.from(params.__ow_body, 'base64').toString('utf-8');

    // Verify the GitHub webhook signature
    const isSignatureValid = await verifySignature(
      params.GITHUB_SECRET,
      params.__ow_headers["x-hub-signature-256"],
      body
    );

    if (!isSignatureValid) {
      // GitHub signature verification failed
      return errorResponse(401, "Unauthorized", logger);
    } else {
      logger.debug("Signature verified");
    }

    // Parse the request body as JSON so its data is useful in the action
    const githubParams = JSON.parse(body) || {};

    // Optionally, merge the GitHub webhook request parameters with the action parameters
    const mergedParams = {
      ...params,
      ...githubParams
    };

    // Do work based on the GitHub webhook request
    doWork(mergedParams);

    return {
      statusCode: 200,
      body: { message: "GitHub webhook received and processed!" }
    };

  } catch (error) {
    // Log any server errors
    logger.error(error);
    // Return with 500 status code
    return errorResponse(500, "Server error", logger);
  }
}
```

## GitHub での Webhook の設定

GitHub.comに戻り、webhook を作成する際に、GitHub.comに同じシークレット値を指定します。 で指定されたシークレットの値を使用 `.env` ファイルの `GITHUB_SECRET` キー。

GitHub.comで、リポジトリ設定に移動し、webhook を編集します。 Webhook 設定で、にシークレット値を指定します `Secret` フィールド。 クリック __Webhook を更新__ 下部で変更を保存します。

![Github Webhook 秘密鍵](./assets/github-webhook-verification/github-webhook-settings.png)

次の手順に従うと、App Builder アクションで、受信 Webhook リクエストが実際にGitHub.com Webhook からのものであることを安全に検証できます。
