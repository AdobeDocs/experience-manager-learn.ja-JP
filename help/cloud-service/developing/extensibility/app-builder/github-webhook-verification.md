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
exl-id: 5492dc7b-f034-4a7f-924d-79e083349e26
source-git-commit: 8f64864658e521446a91bb4c6475361d22385dc1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Github.com webhook の検証

Webhook を使用すると、GitHub.comの特定のイベントを購読する統合を作成または設定できます。 これらのイベントのいずれかがトリガーされると、GitHub は HTTP POSTペイロードを Webhook の設定済み URL に送信します。 ただし、セキュリティ上の理由から、受信する Webhook リクエストが実際には悪意のあるアクターからではなく、GitHub からのものであることを確認することが重要です。 このチュートリアルでは、共有暗号鍵を使用して、AdobeのApp Builder アクションでGitHub.com webhook リクエストを検証する手順を説明します。

## AppBuilder での Github シークレットの設定

1. **ファイルにシークレット `.env` 追加：**

   App Builder プロジェクトの `.env` ファイルに、GitHub.com webhook シークレットのカスタムキーを追加します。

   ```env
   # Specify your secrets here
   # This file must not be committed to source control
   ...
   GITHUB_SECRET=my-github-webhook-secret-1234!
   ```

2. **ファイル `ext.config.yaml` 更新：**

   GitHub.com Webhook リクエストを検証するには、`ext.config.yaml` ファイルを更新する必要があります。

   - GitHub.comから生のリクエスト本文を受け取るには、AppBuilder アクション `web` の設定を `raw` に設定します。
   - AppBuilder アクション設定の `inputs` の下で、`GITHUB_SECRET` キーを追加し、秘密鍵を含む `.env` フィールドにマッピングします。 このキーの値は、先頭に `$` が付いた `.env` フィールド名です。
   - AppBuilder アクション設定の `require-adobe-auth` 注釈を `false` に設定して、Adobe認証を必要とせずにアクションを呼び出せるようにします。

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

次に、以下に提供されているJavaScript コード（[GitHub.com のドキュメント ](https://docs.github.com/en/webhooks/using-webhooks/validating-webhook-deliveries#javascript-example) からコピーしたもの）を AppBuilder アクションに追加します。 必ず `verifySignature` 関数を書き出してください。

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

次に、リクエストヘッダーの署名を `verifySignature` 関数によって生成された署名と比較して、リクエストが GitHub からのものであることを確認します。

AppBuilder アクションの `index.js` で、`main` 関数に次のコードを追加します。


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

GitHub.comに戻り、webhook を作成する際に、GitHub.comに同じシークレット値を指定します。 `.env` ファイルの `GITHUB_SECRET` キーで指定されたシークレット値を使用します。

GitHub.comで、リポジトリ設定に移動し、webhook を編集します。 Webhook 設定で、「`Secret`」フィールドにシークレット値を指定します。 下部の __Webhook を更新__ をクリックして、変更を保存します。

![Github Webhook 秘密鍵 ](./assets/github-webhook-verification/github-webhook-settings.png)

次の手順に従うと、App Builderのアクションで、受信 Webhook リクエストが実際にGitHub.com Webhook からのものであることを安全に確認できます。