---
title: アセットの書き出し
description: ローカルマシンにアセットを一括で書き出し、ダウンロードする方法について説明します。
feature: Asset Management
version: Cloud Service
topic: Content Management
role: Developer
level: Experienced
last-substantial-update: 2024-04-08T00:00:00Z
doc-type: Tutorial
jira: KT-15313
thumbnail: KT-15313.jpeg
exl-id: d04c3316-6f8f-4fd1-9df1-3fe09d44f735
duration: 256
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '517'
ht-degree: 100%

---

# アセットの書き出し

カスタマイズ可能な Node.js スクリプトを使用して、ローカルマシンにアセットを書き出す方法について説明します。 この書き出しスクリプトは、[AEM Assets HTTP API](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) を使用してプログラムで AEM からアセットをダウンロードする方法の例を示します。特に元のレンディションに焦点を当てて最高品質を実現します。 AEM Assets のフォルダー構造をローカルドライブにレプリケートするように設計されているので、アセットのバックアップや移行が簡単になります。

XMP としてアセットにメタデータが埋め込まれていない限り、スクリプトは関連するメタデータなしでアセットの元のレンディションのみをダウンロードします。 つまり、AEM に保存されているがアセットファイルに統合されていない説明的な情報、分類またはタグは、ダウンロードには含まれません。 スクリプトを変更して含めることで、その他のレンディションもダウンロードできます。 書き出したアセットを保存できるだけのスペースがあることを確認します。

スクリプトは通常、AEM オーサーに対して実行されますが、AEM Assets HTTP API エンドポイントとアセットレンディションが Dispatcher 経由でアクセスできる限り、AEM パブリッシュに対しても実行できます。

スクリプトを実行する前に、AEM インスタンスの URL、ユーザー資格情報（アクセストークン）、書き出すフォルダーへのパスを使用してスクリプトを設定する必要があります。

## 書き出しスクリプト

JavaScript モジュールとして記述されたスクリプトは、`node-fetch` への依存関係があるので、Node.js プロジェクトの一部です。 [プロジェクトを zip ファイルとしてダウンロード](./assets/export/export-aem-assets-script.zip)するか、以下のスクリプトをタイプ `module` の空の Node.js プロジェクトにコピーし、`npm install node-fetch` を実行して依存関係をインストールできます。

このスクリプトは、AEM Assets フォルダーツリーを移動し、アセットとフォルダーをマシン上のローカルフォルダーにダウンロードします。 [AEM Assets HTTP API](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/assets/admin/mac-api-assets) を使用してフォルダーとアセットデータを取得し、アセットの元のレンディションをダウンロードします。

```javascript
// export-assets.js

import fetch from 'node-fetch';
import { promises as fs } from 'fs';
import path from 'path';

// Do not process the contents of these well-known AEM system folders
const SKIP_FOLDERS = ['/content/dam/appdata', '/content/dam/projects', '/content/dam/_CSS', '/content/dam/_DMSAMPLE' ];

/**
 * Determine if the folder should be processed based on the entity and AEM path.
 * 
 * @param {Object} entity the AEM entity that should represent a folder returned from AEM Assets HTTP API
 * @param {String} aemPath the path in AEM of this source
 * @returns true if the entity should be processed, false otherwise
 */
function isValidFolder(entity, aemPath) {
    if (aemPath === '/content/dam') {
        // Always allow processing /content/dam 
        return true;
    } else if (!entity.class.includes('assets/folder')) {
        return false;
    } if (SKIP_FOLDERS.find((path) => path === aemPath)) {
        return false;
    } else if (entity.properties.hidden) {
        return false;
    }
    
    return true;
}

/**
 * Determine if the entity is downloadable.
 * @param {Object} entity the AEM entity that should represent an asset returned from AEM Assets HTTP API
 * @returns true if the entity is downloadable, false otherwise
 */
function isDownloadable(entity) {
    if (entity.class.includes('assets/folder')) {
        return false;
    } else if (entity.properties.contentFragment) {
        return false;
    }

    return true;
}

/**
 * Helper function to get the link from the entity based on the relationship name.
 * @param {Object} entity the entity from the AEM Assets HTTP API
 * @param {String} rel the relationship name
 * @returns 
 */
function getLink(entity, rel) {
    return entity.links.find(link => link.rel.includes(rel));
}

/**
 * Helper function to fetch JSON data from the AEM Assets HTTP API.
 * @param {String} url the AEM Assets HTTP API URL to fetch data from
 * @returns the JSON response of the AEM Assets HTTP API
 */
async function fetchJSON(url) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
            'Content-Type': 'application/json'
        }
    });

    if (!response.ok) {
        throw new Error(`Error: ${response.status}`);
    }

    return response.json();
}

/**
 * Helper function to download a file from AEM Assets.
 * @param {String} url the URL of the asset rendition to download
 * @param {String} outputPath the local path to save the downloaded file
 */
async function downloadFile(url, outputPath) {
    const response = await fetch(url, {
        method: 'GET',
        headers: {
            'Authorization': `Bearer ${AEM_ACCESS_TOKEN}`,
        }
    });

    if (!response.ok) {
        throw new Error(`Failed to download file: ${response.statusText}`);
    }

    const arrayBuffer = await response.arrayBuffer();
    await fs.writeFile(outputPath, Buffer.from(arrayBuffer));

    console.log(`Downloaded asset: ${outputPath}`);
}

/**
 * Main entry
 * @param {Object} options the options for downloading assets
 * @param {String} options.folderUrl the URL of the AEM folder to download
 * @param {String} options.localPath the local path to save the downloaded assets
 * @param {String} options.aemPath the AEM path of the folder to download
 */
async function downloadAssets({apiUrl, localPath = LOCAL_DOWNLOAD_FOLDER, aemPath = '/content/dam'}) {    
    if (!apiUrl) {
        // Handle the initial call to the script, which should just provide the AEM path
        // Construct the proper AEM Assets HTTP API URL as it uses a truncated AEM path
        const prefix = "/content/dam/";
        let apiPath = aemPath.startsWith(prefix) ? aemPath.substring(prefix.length) : aemPath;    

        if (!apiPath.startsWith('/')) {
            apiPath = '/' + apiPath;
        }

        apiUrl = `${AEM_HOST}/api/assets.json${apiPath}`
    }
    
    const data = await fetchJSON(apiUrl);
    const entities = data.entities || [];

    // Process folders first
    for (const folder of entities.filter(entity => entity.class.includes('assets/folder'))) {
        const newLocalPath = path.join(localPath, folder.properties.name);
        const newAemPath = path.join(aemPath, folder.properties.name);

        if (!isValidFolder(folder, newAemPath)) {
            continue;
        }

        await fs.mkdir(newLocalPath, { recursive: true });
    
        await downloadAssets({
            apiUrl: getLink(folder, 'self')?.href, 
            localPath: newLocalPath, 
            aemPath: newAemPath
        });
    }

    let downloads = [];

    // Process assets
    for (const asset of entities.filter(entity => entity.class.includes('assets/asset'))) {
        const assetLocalPath = path.join(localPath, asset.properties.name);
        if (isDownloadable(asset)) {
            downloads.push(downloadFile(getLink(asset, 'content')?.href, assetLocalPath));
        }

        // Process in batches of MAX_CONCURRENT_DOWNLOADS
        if (downloads.length >= MAX_CONCURRENT_DOWNLOADS) {
            await Promise.all(downloads);
            downloads = [];
        }
    }

    // Wait for the remaining downloads to finish
    await Promise.all(downloads);
    downloads = [];

    // Handle pagination
    const nextUrl = getLink(data, 'next');
    if (nextUrl) {
        await downloadAssets({
            apiUrl: nextUrl?.href,
            localPath,
            aemPath
        });
    }
}

/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './exported-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;

/***** SCRIPT ENTRY POINT *****/

console.time('Download AEM assets');

await downloadAssets({
    aemPath: AEM_ASSETS_FOLDER, 
    localPath: LOCAL_DOWNLOAD_FOLDER
}).catch(console.error);

console.timeEnd('Download AEM assets');
```

## 書き出しの設定

スクリプトをダウンロードしたら、スクリプトの下部にある設定変数を更新します。

`AEM_ACCESS_TOKEN` を入手するには、[AEM as a Cloud Service に対するトークンベースの認証](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview)チュートリアルの手順を使用します。 多くの場合、書き出しの完了に要する時間が 24 時間未満で、トークンを生成するユーザーが書き出すアセットへの読み取りアクセス権を持っている限り、24 時間の開発者トークンで十分です。

```javascript
...
/***** SCRIPT CONFIGURATION *****/

// AEM host is the URL of the AEM environment to download the assets from
const AEM_HOST = 'https://author-p123-e456.adobeaemcloud.com';

// AEM access token used to access the AEM host. 
// This access token must have read access to the folders and assets to download.
const AEM_ACCESS_TOKEN = "eyJhbGciOiJS...zCprYZD0rSjg6g";

// The root folder in AEM to download assets from.
const AEM_ASSETS_FOLDER = '/content/dam/wknd-shared';

// The local folder to save the downloaded assets.
const LOCAL_DOWNLOAD_FOLDER = './export-assets';

// The number of maximum concurrent downloads to avoid overwhelming the client or server. 10 is typically a good value.
const MAX_CONCURRENT_DOWNLOADS = 10;
```

## アセットの書き出し

Node.js を使用してスクリプトを実行し、アセットをローカルマシンに書き出します。

アセットの数とサイズによっては、スクリプトの完了に時間がかかる場合があります。 スクリプトが実行されると、[進行状況](#output)をコンソールに記録します。

```shell
$ node export-assets.js
```

## 出力の書き出し

書き出しスクリプトは、進行状況をコンソールに記録し、ダウンロード中のアセットを示します。 スクリプトが完了すると、アセットは設定で指定されたローカルフォルダーに保存され、アセットのダウンロードにかかった合計時間がログの最後に記録されます。

```plaintext
...
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring3sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring5sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/skitouring/skitouring6sjoeberg.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/wa_camping_adobe.pdf
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobestock-156407519.jpeg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3094.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-mg-3851.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a7083.jpg
Downloaded asset: exported-assets/wknd-shared/en/magazine/western-australia/adobe-waadobe-wa-b6a6978.jpg
Download AEM assets: 24.770s
```

書き出されたアセットは、設定 `LOCAL_DOWNLOAD_FOLDER` で指定されたローカルフォルダーにあります。 フォルダー構造は AEM Assets のフォルダー構造と同じで、適切なサブフォルダーにアセットがダウンロードされます。 これらのファイルは、他の AEM インスタンスへの[一括読み込み](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/migration/bulk-import)やバックアップ用に、[サポートされるクラウドストレージプロバイダー](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/assets/assets-view/bulk-import-assets-view)にアップロードできます。

![書き出されたアセット](./assets/export/exported-assets.png)
