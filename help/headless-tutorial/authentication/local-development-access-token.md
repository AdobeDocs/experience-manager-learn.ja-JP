---
title: ローカル開発アクセストークン
description: AEMローカル開発アクセストークンは、HTTP を介して AEM オーサーまたはパブリッシュサービスとプログラム的にやり取りするAEM as a Cloud Serviceとの統合の開発を高速化するために使用されます。
version: Cloud Service
doc-type: tutorial
topics: Development, Security
feature: APIs
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: Headless, Integrations
role: Developer
level: Intermediate, Experienced
last-substantial-update: 2023-01-12T00:00:00Z
exl-id: 197444cb-a68f-4d09-9120-7b6603e1f47d
source-git-commit: 8b6d8d99c806e782a1ddce2b300211f8d4c9da56
workflow-type: tm+mt
source-wordcount: '1067'
ht-degree: 0%

---

# ローカル開発アクセストークン

AEM as a Cloud Serviceにプログラム的にアクセスする必要がある統合を構築する開発者は、ローカル開発アクティビティを容易にするために、AEMの一時的なアクセストークンを簡単かつ迅速に取得する必要があります。 このニーズを満たすために、AEM開発者コンソールを使用して、AEMにプログラム的にアクセスするために使用できる一時的なアクセストークンを自己生成できます。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## ローカル開発アクセストークンの生成

![ローカル開発アクセストークンの取得](assets/local-development-access-token/getting-a-local-development-access-token.png)

ローカル開発アクセストークンを使用すると、トークンを生成したユーザーとしての AEM オーサーサービスおよびパブリッシュサービスへのアクセス権と、その権限を提供できます。 これは開発トークンですが、このトークンを共有しないでください。または、ソース管理下に保存してください。

1. In [Adobe Admin Console](https://adminconsole.adobe.com/) 開発者は、次のメンバーであることを確認します。
   + __Cloud Manager — 開発者__ IMS 製品プロファイル (AEM Developer Console へのアクセス権を付与 )
   + 次のいずれか __AEM Administrators__ または __AEM Users__ アクセストークンが統合されるAEM環境のサービス用の IMS 製品プロファイル
   + Sandbox AEMas a Cloud Serviceの環境では、 __AEM Administrators__ または __AEM Users__ 製品プロファイル
1. にログインします。 [AdobeCloud Manager](https://my.cloudmanager.adobe.com)
1. 統合するAEMas a Cloud Service環境を含むプログラムを開きます。
1. 次をタップします。 __省略記号__ の環境の隣に __環境__ セクションを選択し、 __開発者コンソール__
1. 次の __統合__ タブ
1. 次をタップします。 __ローカルトークン__ タブ
1. タップ __ローカル開発トークンを取得__ ボタン
1. をタップします。 __ダウンロードボタン__ 左上隅に、 `accessToken` の値を指定して、JSON ファイルを開発マシン上の安全な場所に保存します。
   + これは、AEMas a Cloud Service環境への 24 時間の開発者アクセストークンです。

![AEM Developer Console — 統合 —ローカル開発トークンの取得](./assets/local-development-access-token/developer-console.png)

## ローカル開発アクセストークンを使用{#use-local-development-access-token}

![ローカル開発アクセストークン — 外部アプリケーション](assets/local-development-access-token/local-development-access-token-external-application.png)

1. AEM Developer Console から一時的なローカル開発アクセストークンをダウンロードします。
   + ローカル開発のアクセストークンは 24 時間ごとに期限が切れるので、開発者は新しいアクセストークンを毎日ダウンロードする必要があります
1. AEM as a Cloud Serviceとプログラム的にやり取りする外部アプリケーションが開発中です
1. 外部アプリケーションがアクセストークンをローカル開発で読み取る
1. 外部アプリケーションは、AEMas a Cloud Serviceに対する HTTP リクエストを作成し、ローカル開発アクセストークンを Bearer トークンとして HTTP リクエストの Authorization ヘッダーに追加します。
1. AEM as a Cloud Serviceは、HTTP リクエストを受信し、リクエストを認証し、HTTP リクエストによってリクエストされた作業を実行し、HTTP レスポンスを外部アプリケーションに返します

### サンプル外部アプリケーション

簡単な外部 JavaScript アプリケーションを作成し、ローカル開発者アクセストークンを使用して、HTTPS 経由でas a Cloud Serviceにプログラム的にアクセスする方法を説明します。 これは、次の方法を示します。 _任意_ AEM以外で実行されるアプリケーションまたはシステムは、フレームワークや言語に関係なく、アクセストークンを使用して、AEM as a Cloud Serviceに対するプログラム認証とアクセスを行うことができます。 内 [次のセクション](./service-credentials.md)を使用する場合は、実稼動用のトークンを生成する方法をサポートするように、このアプリケーションコードを更新します。

このサンプルアプリケーションはコマンドラインから実行され、次のフローを使用して、AEM Assets HTTP API を使用してAEMのアセットメタデータを更新します。

1. コマンドラインからパラメータを読み込みます (`getCommandLineParams()`)
1. AEMas a Cloud Service(`getAccessToken(...)`)
1. コマンドラインパラメーター (`listAssetsByFolder(...)`)
1. リストされているアセットのメタデータを、コマンドラインパラメーター (`updateMetadata(...)`)

アクセストークンを使用してAEMをプログラムで認証する際のキー要素は、次の形式で、AEMに対しておこなわれたすべての HTTP リクエストに Authorization HTTP リクエストヘッダーを追加します。

+ `Authorization: Bearer ACCESS_TOKEN`

## 外部アプリケーションの実行

1. 以下を確認します。 [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js) は、外部ローカル開発の実行に使用される、アプリケーションマシンにインストールされています
1. をダウンロードして展開します。 [サンプル外部アプリケーション](./assets/aem-guides_token-authentication-external-application.zip)
1. コマンドラインから、このプロジェクトのフォルダでを実行します。 `npm install`
1. を [ダウンロードしたローカル開発アクセストークン](#download-local-development-access-token) 次の名前のファイルに `local_development_token.json` プロジェクトのルートにある
   + ただし、Git に認証情報をコミットしないでください。
1. 開く `index.js` 外部アプリケーションのコードとコメントを確認します。

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
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#retrieve-a-folder-listing
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
   * https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/admin/mac-api-assets.html?lang=en#update-asset-metadata
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

   以下を確認します。 `fetch(..)` の呼び出し `listAssetsByFolder(...)` および `updateMetadata(...)`、および `headers` を定義します。 `Authorization` 値がの HTTP リクエストヘッダー `Bearer ACCESS_TOKEN`. 外部アプリケーションからの HTTP リクエストは、AEM as a Cloud Serviceに対してどのように認証されます。

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

   AEM as a Cloud Serviceへの HTTP リクエストは、Authorization ヘッダーに Bearer アクセストークンを設定する必要があります。 各AEMas a Cloud Service環境には、独自のアクセストークンが必要です。 開発のアクセストークンはステージまたは実稼動では機能せず、ステージのアクセストークンは開発または実稼動では機能せず、実稼動は開発またはステージでは機能しません。

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

   + `aem`:アプリケーションがやり取りするAEMas a Cloud Service環境のスキームとホスト名 ( 例： `https://author-p1234-e5678.adobeaemcloud.com`) をクリックします。
   + `folder`:アセットが `propertyValue`;追加しない `/content/dam` プレフィックス ( 例： `/wknd-shared/en/adventures/napa-wine-tasting`)
   + `propertyName`:更新するアセットプロパティの名前（相対的な名前） `[dam:Asset]/jcr:content` ( 例： `metadata/dc:rights`) をクリックします。
   + `propertyValue`:設定する値 `propertyName` からスペースを含む値は、 `"` ( 例： `"WKND Limited Use"`)
   + `file`:AEM Developer Console からダウンロードした JSON ファイルの相対ファイルパス。

   更新された各アセットのアプリケーション結果出力が正常に実行された場合：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd-shared/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### AEMでのメタデータの更新の検証

AEM as a Cloud Service環境にログインして、メタデータが更新されたことを確認します ( 同じホストが `aem` コマンドラインパラメータにアクセスします )。

1. 外部アプリケーションがやり取りするAEMas a Cloud Service環境にログインします ( `aem` コマンドラインパラメータ )
1. 次に移動： __Assets__ > __ファイル__
1. これにより、 `folder` コマンドラインパラメータ（例： ） __WKND__ > __英語__ > __冒険__ > __ナパワインの試飲会__
1. を開きます。 __プロパティ__ フォルダー内の（コンテンツフラグメント以外の）アセット
1. をタップして、 __詳細__ タブ
1. 更新されたプロパティの値を確認します（例： ）。 __著作権__ 更新された `metadata/dc:rights` JCR プロパティ。 `propertyValue` パラメーター、例： __WKND 制限付き使用__

![WKND 制限付きメタデータ更新を使用](./assets/local-development-access-token/asset-metadata.png)

## 次の手順

これで、ローカル開発トークンを使用してプログラムからAEM as a Cloud Serviceにアクセスできました。 次に、サービス資格情報を使用して処理するようにアプリケーションを更新する必要があります。これにより、このアプリケーションを実稼動コンテキストで使用できます。

+ [サービス資格情報の使用方法](./service-credentials.md)
