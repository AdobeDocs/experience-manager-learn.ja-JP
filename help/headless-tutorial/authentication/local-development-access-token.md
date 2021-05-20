---
title: ローカル開発アクセストークン
description: AEMローカル開発アクセストークンは、HTTPを介してプログラムによってAEMオーサーまたはパブリッシュサービスとやり取りするCloud ServiceとしてAEMとの統合の開発を高速化するために使用されます。
version: cloud-service
doc-type: tutorial
topics: Development, Security
feature: API
activity: develop
audience: developer
kt: 6785
thumbnail: 330477.jpg
topic: ヘッドレス、統合
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1073'
ht-degree: 0%

---


# ローカル開発アクセストークン

AEM as a Cloud Serviceへのプログラム的なアクセスを必要とするローカル開発を構築する開発者は、統合アクティビティを容易にするために、AEMの一時的なアクセストークンを簡単かつ迅速に取得する必要があります。 このニーズを満たすために、AEM開発者コンソールを使用して、AEMにプログラム的にアクセスするために使用できる一時的なアクセストークンを自己生成できます。

>[!VIDEO](https://video.tv.adobe.com/v/330477/?quality=12&learn=on)

## ローカル開発アクセストークンの生成

![ローカル開発アクセストークンの取得](assets/local-development-access-token/getting-a-local-development-access-token.png)

ローカル開発アクセストークンは、トークンを生成したユーザーとしてのAEMオーサーサービスとパブリッシュサービスへのアクセス権と、その権限を提供します。 これは開発トークンですが、このトークンを共有しないでください。また、ソース管理下に保存してください。

1. [AdobeのAdminConsole](https://adminconsole.adobe.com/)で、開発者が以下のメンバーであることを確認します。
   + __Cloud Manager - DeveloperIMS製__ 品プロファイル(AEM Developer Consoleへのアクセス権を付与)
   + アクセストークンが統合されるAEM環境のサービス用の&#x200B;__AEM Administrators__&#x200B;または&#x200B;__AEM Users__ IMS製品プロファイル
   + Cloud Service環境としてのSandbox AEMは、__AEM Administrators__&#x200B;または&#x200B;__AEM Users__&#x200B;製品プロファイルのメンバーシップのみ必要です
1. [AdobeCloud Manager](https://my.cloudmanager.adobe.com)にログインします。
1. と統合するAEM環境としてのCloud Serviceを含むプログラムを開きます。
1. 「__環境__」セクションで環境の横にある&#x200B;__省略記号__&#x200B;をタップし、「__開発者コンソール__」を選択します。
1. 「__統合__」タブをタップします。
1. 「__ローカル開発トークンを取得__」ボタンをタップします
1. 左上隅の&#x200B;__ダウンロードボタン__&#x200B;をタップして、`accessToken`値を含むJSONファイルをダウンロードし、開発マシン上の安全な場所にJSONファイルを保存します。
   + これは、AEM as a Cloud Service環境への24時間の開発者アクセストークンです。

![AEM開発者コンソール — 統合 —ローカル開発トークンの取得](./assets/local-development-access-token/developer-console.png)

## ローカル開発アクセストークン{#use-local-development-access-token}を使用

![ローカル開発アクセストークン — 外部アプリケーション](assets/local-development-access-token/local-development-access-token-external-application.png)

1. AEM開発者コンソールから一時的なローカル開発アクセストークンをダウンロードします。
   + ローカル開発のアクセストークンは24時間ごとに期限が切れるので、開発者は新しいアクセストークンを毎日ダウンロードする必要があります
1. プログラムによってAEM as a Cloud Serviceとやり取りする外部アプリケーションが開発されている
1. 外部アプリケーションがローカル開発アクセストークンを読み取る
1. 外部アプリケーションは、AEMにCloud ServiceとしてHTTPリクエストを構築し、ローカル開発アクセストークンをBearerトークンとしてHTTPリクエストのAuthorizationヘッダーに追加します
1. AEM as aCloud ServiceはHTTPリクエストを受信し、リクエストを認証し、HTTPリクエストによってリクエストされた作業を実行し、HTTPレスポンスを外部アプリケーションに返します

### サンプル外部アプリケーション

簡単な外部JavaScriptアプリケーションを作成し、ローカル開発者アクセストークンを使用して、HTTPS経由でCloud ServiceとしてAEMにプログラム的にアクセスする方法を説明します。 この例は、フレームワークや言語に関係なく、AEMの外部で動作する&#x200B;_あらゆる_&#x200B;アプリケーションやシステムが、AEMをプログラムによる認証やCloud Serviceとしてアクセスする際に、どのように使用できるかを示しています。 [次の節](./service-credentials.md)では、本番用のトークンを生成する方法をサポートするために、このアプリケーションコードを更新します。

このサンプルアプリケーションはコマンドラインから実行され、次のフローを使用してAEM Assets HTTP APIを使用してAEMアセットメタデータを更新します。

1. コマンドライン(`getCommandLineParams()`)からパラメータを読み込む
1. AEMに対する認証に使用するアクセストークンをCloud Service(`getAccessToken(...)`)として取得します
1. コマンドラインパラメーター(`listAssetsByFolder(...)`)で指定されたAEMアセットフォルダー内のすべてのアセットを一覧表示します。
1. リストされているアセットのメタデータを、コマンドラインパラメーター(`updateMetadata(...)`)で指定された値に更新する

アクセストークンを使用してAEMをプログラムで認証する際の重要な要素は、次の形式で、AEMに対しておこなわれたすべてのHTTP要求にAuthorization HTTP要求ヘッダーを追加することです。

+ `Authorization: Bearer ACCESS_TOKEN`

## 外部アプリケーションの実行

1. [Node.js](/help/cloud-service/local-development-environment/development-tools.md?lang=en#node-js)がローカル開発マシンにインストールされ、外部アプリケーションの実行に使用されていることを確認します。
1. [サンプル外部アプリケーション](./assets/aem-guides_token-authentication-external-application.zip)をダウンロードして解凍します。
1. コマンドラインから、このプロジェクトのフォルダーで`npm install`を実行します。
1. ダウンロードした[ローカル開発アクセストークン](#download-local-development-access-token)を、プロジェクトのルートにある`local_development_token.json`という名前のファイルにコピーします。
   + しかし、Gitに資格情報をコミットしないでください。
1. `index.js`を開き、外部アプリケーションのコードとコメントを確認します。

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
       console.log('Example usage: node index.js aem=https://author-p1234-e5678.adobeaemcloud.com propertyName=metadata/dc:rights "propertyValue=WKND Limited Use" folder=/wknd/en/adventures/napa-wine-tasting file=credentials-file.json' );
   
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
   *              Example: '/wknd/en/adventures/napa-wine-tasting'
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

   `listAssetsByFolder(...)`と`updateMetadata(...)`の`fetch(..)`の呼び出しを確認し、`headers`は`Bearer ACCESS_TOKEN`という値で`Authorization` HTTPリクエストヘッダーを定義します。 外部アプリケーションからのHTTPリクエストは、Cloud ServiceとしてAEMに認証されます。

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

   AEMにCloud ServiceとしてHTTPリクエストを送信する場合、はAuthorizationヘッダーにBearerアクセストークンを設定する必要があります。 各AEM as a Cloud Service環境には、独自のアクセストークンが必要です。 開発のアクセストークンはステージまたは実稼動では機能せず、ステージのアクセストークンは開発または実稼動では機能せず、実稼動は開発またはステージでは機能しません。

1. コマンドラインを使用して、プロジェクトのルートからアプリケーションを実行し、次のパラメーターを渡します。

   ```shell
   $ node index.js \
       aem=https://author-p1234-e5678.adobeaemcloud.com \
       folder=/wknd/en/adventures/napa-wine-tasting \
       propertyName=metadata/dc:rights \
       propertyValue="WKND Limited Use" \
       file=local_development_token.json
   ```

   次のパラメーターが渡されます。

   + `aem`:アプリケーションがやり取りするCloud Service環境としてのAEMのスキームとホスト名(例： `https://author-p1234-e5678.adobeaemcloud.com`)をクリックします。
   + `folder`:アセットがで更新されるアセットフォルダーのパ `propertyValue`ス。プレフィッ `/content/dam` クスを追加しない(例： `/wknd/en/adventures/napa-wine-tasting`)
   + `propertyName`:更新するアセットプロパティの `[dam:Asset]/jcr:content` 名前(例： `metadata/dc:rights`)をクリックします。
   + `propertyValue`:に設定する `propertyName` 値。スペースを含む値は、（例えば）でカプセル化す `"` る必要があります。 `"WKND Limited Use"`)
   + `file`:AEM開発者コンソールからダウンロードしたJSONファイルの相対ファイルパス。

   更新された各アセットのアプリケーション結果出力の正常な実行：

   ```shell
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_277654931.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_239751461.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_280313729.jpg.json
   200 - OK @ https://author-p1234-e5678.adobeaemcloud.com/api/assets/wknd/en/adventures/napa-wine-tasting/AdobeStock_286664352.jpg.json
   ```

### AEMでのメタデータの更新の検証

AEM as aCloud Service環境にログインして、メタデータが更新されていることを確認します（`aem`コマンドラインパラメーターに渡された同じホストにアクセスする必要があります）。

1. 外部アプリケーションが操作するCloud Service環境としてAEMにログインします（`aem`コマンドラインパラメーターで指定したのと同じホストを使用）。
1. __アセット__ / __ファイル__&#x200B;に移動します。
1. `folder`コマンドラインパラメーターで指定されたアセットフォルダーに移動します（例：__WKND__ > __English__ > __Adventures__ > __Napa Wine Tasting__）。
1. フォルダー内の任意の（コンテンツフラグメント以外の）アセットの&#x200B;__プロパティ__&#x200B;を開きます
1. 「__詳細__」タブをタップします。
1. 更新されたプロパティの値を確認します。例えば、__WKND Limited Use__&#x200B;のように、更新された`metadata/dc:rights` JCRプロパティにマッピングされた&#x200B;__Copyright__&#x200B;の値を確認します。`propertyValue`パラメーターで指定された値を反映します。

![WKND Limited Use Metadata Update](./assets/local-development-access-token/asset-metadata.png)

## 次の手順

ローカル開発トークンを使用してプログラムでAEMにCloud Serviceとしてアクセスしたので、サービス資格情報を使用して処理するようにアプリケーションを更新する必要があります。そのため、このアプリケーションを実稼動コンテキストで使用できます。

+ [サービス資格情報の使用方法](./service-credentials.md)
