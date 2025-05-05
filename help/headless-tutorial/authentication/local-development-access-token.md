---
title: ローカル開発アクセストークン
description: AEM ローカル開発アクセストークンは、HTTP を介して AEM オーサーまたはパブリッシュサービスとプログラムでやり取りする AEM as a Cloud Service との統合の開発を高速化するために使用されます。
version: Experience Manager as a Cloud Service
feature: APIs
jira: KT-6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
doc-type: Tutorial
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
duration: 572
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1067'
ht-degree: 100%

---

# ローカル開発アクセストークン

AEM as a Cloud Service にプログラムでアクセスする必要がある統合を構築する開発者は、ローカル開発アクティビティを容易にするために、AEM の一時的なアクセストークンを容易かつ迅速に取得する必要があります。このニーズを満たすために、AEM Developer Console を使用して、AEM にプログラムでアクセスするために使用できる一時的なアクセストークンを自己生成できます。

>[!VIDEO](https://video.tv.adobe.com/v/330477?quality=12&learn=on)

## ローカル開発アクセストークンの生成

![ローカル開発アクセストークンの取得](assets/local-development-access-token/getting-a-local-development-access-token.png)

ローカル開発アクセストークンを使用すると、トークンを生成したユーザーとして AEM オーサーサービスおよびパブリッシュサービスにアクセスできるようになると共に、そのサービスの権限も取得できます。これは開発用トークンですが、このトークンを共有したり、ソースコントロールに保存したりしないでください。

1. [Adobe Admin Console](https://adminconsole.adobe.com/) で、開発者が以下に属していることを確認します。
   + __Cloud Manager - 開発者__ IMS 製品プロファイル（AEM Developer Console へのアクセス権を付与）
   + アクセストークンが連携する AEM 環境のサービスの __AEM 管理者__&#x200B;または __AEM ユーザー__ IMS 製品プロファイルのどちらか
   + Sandbox AEM as a Cloud Service 環境では、__AEM 管理者__&#x200B;または __AEM ユーザー__&#x200B;製品プロファイルのいずれかに属していることが必要
1. [Adobe Cloud Manager](https://my.cloudmanager.adobe.com) にログインします。
1. 統合先の AEM as a Cloud Service 環境を含んだプログラムを開きます。
1. 「__環境__」セクションにある環境の横の&#x200B;__省略記号__&#x200B;をタップして、__Developer Console__ を選択します。
1. 「__統合__」タブをタップします。
1. 「__ローカルトークン__」タブをタップします。
1. 「__ローカル開発トークンを取得__」ボタンをタップします。
1. 左上の「__ダウンロード__」ボタンをタップして、`accessToken` 値を含んだ JSON ファイルをダウンロードし、開発マシンの安全な場所に保存します。
   + これは、AEM as a Cloud Service 環境への 24 時間有効な開発者アクセストークンです。

![AEM Developer Console - 統合 - ローカル開発トークンの取得](./assets/local-development-access-token/developer-console.png)

## ローカル開発アクセストークンの使用{#use-local-development-access-token}

![ローカル開発アクセストークン - 外部アプリケーション](assets/local-development-access-token/local-development-access-token-external-application.png)

1. AEM Developer Console から一時的なローカル開発アクセストークンをダウンロードします。
   + ローカル開発のアクセストークンは 24 時間ごとに期限が切れるので、開発者は新しいアクセストークンを毎日ダウンロードする必要があります。
1. AEM as a Cloud Service とプログラムでやり取りする外部アプリケーションを開発します。
1. 外部アプリケーションがアクセストークンをローカル開発で読み取ります。
1. 外部アプリケーションが、AEM as a Cloud Service に対する HTTP リクエストを作成し、ローカル開発アクセストークンをベアラートークンとして HTTP リクエストの Authorization ヘッダーに追加します。
1. AEM as a Cloud Service が、HTTP リクエストを受信して認証し、HTTP リクエストで要求された作業を実行し、HTTP レスポンスを外部アプリケーションに返します。

### サンプル外部アプリケーション

シンプルな外部 JavaScript アプリケーションを作成し、ローカル開発者アクセストークンを使用して、HTTPS で AEM as a Cloud Service にプログラムでアクセスする方法を説明します。これにより、AEM の外部で動作する&#x200B;_任意_&#x200B;のアプリケーションまたはシステムは、フレームワークや言語に関係なく、アクセストークンを使用して AEM as a Cloud Service に対する認証とアクセスをプログラムで行うことができます。[次の節](./service-credentials.md)では、実稼働用のトークンを生成するためのアプローチに対応するように、このアプリケーションコードを更新します。

このサンプルアプリケーションはコマンドラインから実行され、次のフローで、AEM Assets HTTP API を使用して AEM のアセットメタデータを更新します。

1. コマンドラインからパラメーターを読み取ります（`getCommandLineParams()`）。
1. AEM as a Cloud Service の認証に使用するアクセストークンを取得します（`getAccessToken(...)`）。
1. コマンドラインパラメーターで指定された AEM アセットフォルダー内のすべてのアセットをリストアップします（`listAssetsByFolder(...)`）。
1. リストアップされたアセットのメタデータを、コマンドラインパラメーターで指定された値で更新します（`updateMetadata(...)`）。

アクセストークンを使用して AEM への認証をプログラムで行う際の重要な要素は、AEM に対して行われるすべての HTTP リクエストに以下の形式で Authorization HTTP リクエストヘッダーを追加することです。

+ `Authorization: Bearer ACCESS_TOKEN`

## 外部アプリケーションの実行

1. 外部アプリケーションの実行に使用するローカル開発マシンに、[Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) がインストールされていることを確認します。
1. [サンプル外部アプリケーション](./assets/aem-guides_token-authentication-external-application.zip)をダウンロードし解凍します。
1. コマンドラインから、このプロジェクトのフォルダーで `npm install` を実行します。
1. [ダウンロードしたローカル開発アクセストークン](#download-local-development-access-token)をプロジェクトのルートにある `local_development_token.json` ファイルにコピーします。
   + ただし、Git に資格情報をコミットしないでください。
1. `index.js` を開き、外部アプリケーションのコードとコメントを確認します。

   ```javascript
   const fetch = require('node-fetch');
   const fs = require('fs');
   const auth = require('@adobe/jwt-auth');
   
   // The root context of the Assets HTTP API
   const ASSETS_HTTP_API = '/api/assets';
   
   // Command line parameters
   let params = { };
   
   /**
   * Application entry point function
   */
   (async () => {
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd-shared/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
       // Parse the command line parameters
       params = getCommandLineParams();
   
       // Set the access token to be used in the HTTP requests to be local development access token
       params.accessToken = await getAccessToken(params.developerConsoleCredentials);
   
       // Get a list of all the assets in the specified assets folder
       let assets = await listAssetsByFolder(params.folder);
   
       // For each asset, update it's metadata
       await assets.forEach(asset => updateMetadata(asset, { 
           [params.propertyName]: params.propertyValue 
       }));
   })();
   
   /**
   * Returns a list of Assets HTTP API asset URLs that reference the assets in the specified folder.
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ja#retrieve-a-folder-listing
   * 
   * @param {*} folder the Assets HTTP API folder path (less the /content/dam path prefix)
   */
   async function listAssetsByFolder(folder) {
       return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
           })
           .then(res => {
               console.log(`${res.status} - ${res.statusText} @ ${params.aem}${ASSETS_HTTP_API}${folder}.json`);
   
               // If success, return the JSON listing assets, otherwise return empty results
               return res.status === 200 ? res.json() : { entities: [] };
           })
           .then(json => { 
               // Returns a list of all URIs for each non-content fragment asset in the folder
               return json.entities
                   .filter((entity) => entity['class'].indexOf('asset/asset') === -1 && !entity.properties.contentFragment)
                   .map(asset => asset.links.find(link => link.rel.find(r => r === 'self')).href);
           });
   }
   
   /**
   * Update the metadata of an asset in AEM
   * 
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=ja#update-asset-metadata
   * 
   * @param {*} asset the Assets HTTP API asset URL to update
   * @param {*} metadata the metadata to update the asset with
   */
   async function updateMetadata(asset, metadata) {        
       await fetch(`${asset}`, {
               method: 'put',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
               body: JSON.stringify({
                   class: 'asset',
                   properties: metadata
               })
           })
           .then(res => { 
               console.log(`${res.status} - ${res.statusText} @ ${asset}`);
           });
   }
   
   /**
   * Parse and return the command line parameters. Expected params are:
   * 
   * - aem = The AEM as a Cloud Service hostname to connect to.
   *              Example: https://author-p12345-e67890.adobeaemcloud.com
   * - folder = The asset folder to update assets in. Note that the Assets HTTP API do NOT use the JCR `/content/dam` path prefix.
   *              Example: '/wknd-shared/en/adventures/napa-wine-tasting'
   * - propertyName = The asset property name to update. Note this is relative to the [dam:Asset]/jcr:content node of the asset.
   *              Example: metadata/dc:rights
   * - propertyValue = The value to update the asset property (specified by propertyName) with.
   *              Example: "WKND Free Use"
   * - file = The path to the JSON file that contains the credentials downloaded from AEM Developer Console
   *              Example: local_development_token_cm_p1234-e5678.json 
   */
   function getCommandLineParams() {
       let parameters = {};
   
       // Parse the command line params, splitting on the = delimiter
       for (let i = 2; i < process.argv.length; i++) {
           let key = process.argv[i].split('=')[0];
           let value = process.argv[i].split('=')[1];
   
           parameters[key] = value;
       };
   
       // Read in the credentials from the provided JSON file
       if (parameters.file) {
           parameters.developerConsoleCredentials = JSON.parse(fs.readFileSync(parameters.file));
       }
   
       console.log(parameters);
   
       return parameters;
   }
   
   async function getAccessToken(developerConsoleCredentials) {s
       if (developerConsoleCredentials.accessToken) {
           // This is a Local Development access token
           return developerConsoleCredentials.accessToken;
       } 
   }
   ```

   `listAssetsByFolder(...)` と `updateMetadata(...)` の `fetch(..)` 呼び出しを確認し、`headers` に `Bearer ACCESS_TOKEN` という値の `Authorization` HTTP リクエストヘッダーが定義されていることを確認します。外部アプリケーションからの HTTP リクエストは、AEM as a Cloud Service に対してこのようにして認証されます。

   ```javascript
   ...
   return fetch(`${params.aem}${ASSETS_HTTP_API}${folder}.json`, {
               method: 'get',
               headers: { 
                   'Content-Type': 'application/json',
                   'Authorization': 'Bearer ' + params.accessToken // Provide the AEM access token in the Authorization header
               },
   })...
   ```

   AEM as a Cloud Service への HTTP リクエストは、Authorization ヘッダーにベアラーアクセストークンを設定する必要があります。AEMas a Cloud Service 環境ごとに、独自のアクセストークンが必要です。開発のアクセストークンはステージングまたは実稼動では機能せず、ステージングのアクセストークンは開発または実稼動では機能しません。また、実稼動のアクセストークンは開発またはステージでは機能しません。

1. コマンドラインを使用して、プロジェクトのルートからアプリケーションを実行し、次のパラメーターを渡します。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd-shared/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   次のパラメーターが渡されます。

   + `aem`：アプリケーションがやり取りする AEM as a Cloud Service 環境のスキームとホスト名（例：`https://author-p1234-e5678.adobeaemcloud.com`）。
   + `folder`：`propertyValue` でアセットが更新されるアセットフォルダーパス。`/content/dam` プレフィックスを追加しないでください（例：`/wknd-shared/en/adventures/napa-wine-tasting`）。
   + `propertyName`：更新するアセットプロパティ名（`[dam:Asset]/jcr:content` からの相対的なパス。例：`metadata/dc:rights`）。
   + `propertyValue`：`propertyName` に設定する値で、スペースを含んだ値は `"` でカプセル化する必要があります（例：`"WKND Limited Use"`）。
   + `file`：AEM Developer Console からダウンロードした JSON ファイルへの相対ファイルパス。

   更新された各アセットのアプリケーション結果の出力が正常に実行された場合は、次のようになります。

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### AEM でのメタデータ更新の確認

AEM as a Cloud Service 環境にログインして、メタデータが更新されていることを確認します（`aem` コマンドラインパラメーターに渡されたホストと同じホストにアクセスします）。

1. 外部アプリケーションがやり取りする AEM as a Cloud Service 環境にログインします（`aem` コマンドラインパラメーターで指定したホストと同じホストを使用します）。
1. __Assets__／__ファイル__&#x200B;に移動します。
1. `folder` コマンドラインパラメーターで指定されたアセットフォルダーに移動します（例：__WKND__／__English__／__Adventures__／__Napa Wine Tasting__）。
1. フォルダー内にある（コンテンツフラグメント以外の）任意のアセットの&#x200B;__プロパティ__&#x200B;を開きます。
1. 「__詳細__」タブをクリックします。
1. 更新されたプロパティの値を確認します（例：__Copyright__）。これは更新された `metadata/dc:rights` JCR プロパティにマップされ、`propertyValue` パラメーターで指定された値を反映しています（例：__WKND Limited Use__）。

![WKND Limited Use メタデータの更新](./assets/local-development-access-token/asset-metadata.png)

## 次の手順

これで、ローカル開発トークンを使用してプログラムで AEM as a Cloud Service にアクセスできました。次は、サービス資格情報の使用に対処するようにアプリケーションを更新して、このアプリケーションを実稼動コンテキストで使用できるようにする必要があります。

+ [サービス資格情報の使用方法](./service-credentials.md)
