---
title: AEM as a Cloud Serviceへのプログラムによるアセットのアップロード
description: '@adobe/aem-upload Node.js ライブラリを使用してAEM as a Cloud Serviceにアセットをアップロードする方法を説明します。'
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 2%

---


# AEM as a Cloud Serviceへのプログラムによるアセットのアップロード

[aem-upload](https://github.com/adobe/aem-upload) Node.js ライブラリを使用するクライアントアプリケーションを使用して、AEM as a Cloud Service環境にアセットをアップロードする方法について説明します。


>[!VIDEO](https://video.tv.adobe.com/v/3476952?quality=12&learn=on)


## 学習内容

このチュートリアルでは、次の内容について説明します。

+ _直接バイナリアップロード_ アプローチを使用して、[aem-upload](https://github.com/adobe/aem-upload) Node.js ライブラリを使用してアセットをAEM as a Cloud Service環境（RDE、開発、ステージ、実稼動）にアップロードする方法。
+ [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) アプリケーションを設定および実行して、AEM as a Cloud Service環境にアセットをアップロードする方法。
+ サンプルアプリケーションコードを確認し、実装の詳細を理解します。
+ AEM as a Cloud Service環境へのプログラムによるアセットのアップロードに関するベストプラクティスを理解します。

## _直接バイナリアップロード_ アプローチについて

_直接バイナリアップロード_ アプローチを使用すると、_事前署名済み URL_ を使用して、AEM as a Cloud Service環境のソースシステムから _クラウドストレージに直接_ ファイルをアップロードできます。 AEMの Java プロセスを使用してバイナリデータをルーティングする必要がなくなり、アップロードの高速化とサーバー負荷の軽減を実現します。

サンプルアプリケーションを実行する前に、直接バイナリアップロードのフローを理解しましょう。

直接バイナリアップロードフローでは、事前署名済み URL を使用してバイナリデータがクラウドストレージに直接アップロードされます。 AEM as a Cloud Serviceは、事前署名済み URL の生成や、アップロード完了に関するAEM Asset Compute サービスへの通知など、軽量の処理を行います。 次の論理フロー図は、直接バイナリアップロードフローを示しています。

![&#x200B; 直接バイナリアップロードフロー &#x200B;](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### aem-upload ライブラリ

[aem-upload](https://github.com/adobe/aem-upload) Node.js ライブラリは、_直接バイナリアップロード_ アプローチの実装の詳細を抽象化します。 アップロードプロセスを調整するために、次の 2 つのクラスが用意されています。

+ **FileSystemUpload** - ディレクトリ構造のサポートなど、ローカルファイルシステムからのファイルのアップロード時に使用します
+ **DirectBinaryUpload** - ストリームまたはバッファーからのアップロードなど、バイナリアップロードプロセスのより詳細な制御に使用します

>[!CAUTION]
>
>Java では、[aem-upload](https://github.com/adobe/aem-upload) ライブラリに相当するものはありません。 クライアントアプリケーションは、_直接バイナリアップロード_ アプローチを使用するために、Node.js で記述する必要があります。 詳しくは、[Experience Manager Assetsの API と操作 &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis) ページを参照してください。

## サンプルアプリケーション

[aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) アプリケーションを使用して、プログラムによるアセットのアップロードプロセスを学びます。 サンプルアプリケーションは、`FileSystemUpload`aem-upload`DirectBinaryUpload` ライブラリから [&#x200B; クラスと &#x200B;](https://github.com/adobe/aem-upload) クラスの両方を使用する方法を示しています。

### 前提条件

サンプルアプリケーションを実行する前に、次の前提条件を満たしていることを確認してください。

+ 迅速な開発環境（RDE）、開発環境などのAEM as a Cloud Service オーサー環境。
+ Node.js （最新の LTS バージョン）
+ Node.js と npm の基本的な理解

>[!CAUTION]
>
> AEM as a Cloud Service SDK（ローカル AEM インスタンス）を使用して、プログラムによるアセットのアップロードプロセスをテストすることはできません。 迅速な開発環境（RDE）、開発環境などのAEM as a Cloud Service環境を使用する必要があります。

### サンプルアプリケーションのダウンロード

1. [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) アプリケーションの zip ファイルをダウンロードして抽出します。

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. 抽出したフォルダーをお気に入りのコードエディターで開きます。

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. コードエディターターミナルを使用して、依存関係をインストールします。

   ```bash
   $ npm install
   ```

   ![&#x200B; サンプルアプリケーション &#x200B;](./assets/programmatic-asset-upload/install-dependencies.png)

### サンプルアプリケーションの設定

サンプルアプリケーションを実行する前に、AEM オーサー URL、_認証方法_ アセットフォルダーパスなど、AEM as a Cloud Service環境に必要な詳細を設定する必要があります。

_aem-upload_ Node.js ライブラリでは [&#x200B; 複数の認証方法 &#x200B;](https://github.com/adobe/aem-upload) がサポートされています。 次の表に、サポートされる _認証方法_ とその目的をまとめます。

| | 基本認証 | [ローカル開発トークン &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [&#x200B; サービス資格情報 &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [OAuth S2S](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [OAuth Web アプリ &#x200B;](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [OAuth SPA](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| はサポートされていますか？ | &check; | &check; | &check; | &cross; | &cross; | &cross; |
| 目的 | ローカル開発 | ローカル開発 | 実稼動 | 該当なし | 該当なし | 該当なし |

サンプルアプリケーションを設定するには、次の手順に従います。

1. `env.example` ファイルを `.env` ファイルにコピーします。

   ```bash
   $ cp env.example .env
   ```

1. `.env` ファイルを開き、AEM as a Cloud Service オーサー URL で `AEM_URL` 環境変数を更新します。

1. 以下のオプションから認証方法を選択し、対応する環境変数を更新します。

>[!BEGINTABS]

>[!TAB 基本認証]

基本認証を使用するには、AEM as a Cloud Serviceでユーザーを作成する必要があります。

1. AEM as a Cloud Service環境にログインします。

1. **ツール**/**セキュリティ**/**ユーザー** に移動し、「**作成**」ボタンをクリックします。

   ![&#x200B; ユーザーを作成 &#x200B;](./assets/programmatic-asset-upload/create-user.png)

1. ユーザーの詳細を入力します

   ![&#x200B; ユーザーの詳細 &#x200B;](./assets/programmatic-asset-upload/user-details.png)

1. 「**グループ**」タブで **DAM ユーザー** グループを追加します。 「**保存して閉じる** ボタンをクリックします。

   ![DAM ユーザーグループの追加 &#x200B;](./assets/programmatic-asset-upload/add-dam-users-group.png)

1. `AEM_USERNAME` および `AEM_PASSWORD` の環境変数を、作成したユーザーのユーザー名とパスワードで更新します。

>[!TAB ローカル開発トークン ]

ローカル開発トークンを取得するには、**AEM** Developer Consoleを使用する必要があります。 生成されたトークンのタイプは JSON Web トークン（JWT）です。

1. [Adobe Cloud Managerにログインし &#x200B;](https://experience.adobe.com/#/@aem/cloud-manager) 目的の **環境** 詳細ページに移動します。 「**」。..「** をクリックし、「**Developer Console**」を選択します。

   ![デベロッパーコンソール](./assets/programmatic-asset-upload/developer-console.png)

1. AEM Developer Consoleにログインし、「_新しいコンソール_ ボタンを使用して新しいコンソールに切り替えます。

1. 「**ツール**」セクションで「**統合**」を選択し、「**ローカルトークンを取得**」ボタンをクリックします。

   ![&#x200B; ローカルトークンを取得 &#x200B;](./assets/programmatic-asset-upload/get-local-token.png)

1. トークン値をコピーし、そのトークン値を使用して `AEM_BEARER_TOKEN` 環境変数を更新します。

ローカル開発トークンは 24 時間有効で、トークンを生成したユーザーに対して発行されます。

>[!TAB  サービス資格情報 ]

サービス資格情報を取得するには、**AEM** Developer Consoleを使用する必要があります。 [jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm モジュールを使用して、JSON web トークン（JWT）タイプのトークンを生成するために使用されます。

1. [Adobe Cloud Managerにログインし &#x200B;](https://experience.adobe.com/#/@aem/cloud-manager) 目的の **環境** 詳細ページに移動します。 「**」。..「** をクリックし、「**Developer Console**」を選択します。

   ![デベロッパーコンソール](./assets/programmatic-asset-upload/developer-console.png)

1. AEM Developer Consoleにログインし、「_新しいコンソール_ ボタンを使用して新しいコンソールに切り替えます。

1. 「**ツール**」セクションで「**統合**」を選択し、「**新規テクニカルアカウントを作成**」ボタンをクリックします。

   ![&#x200B; サービス資格情報の取得 &#x200B;](./assets/programmatic-asset-upload/get-service-credentials.png)

1. 「**表示**」オプションをクリックして、サービス資格情報 JSON をコピーします。

   ![&#x200B; サービス資格情報 &#x200B;](./assets/programmatic-asset-upload/service-credentials.png)

1. サンプルアプリケーションのルートに `service-credentials.json` ファイルを作成し、そのファイルにサービス資格情報 JSON を貼り付けます。

1. service-credentials.json ファイルへのパスを使用して `AEM_SERVICE_CREDENTIALS_FILE` 環境変数を更新します。

1. サービス資格情報のユーザーが、AEM as a Cloud Service環境にアセットをアップロードするために必要な権限を持っていることを確認します。 詳しくは、[AEMでのアクセスの設定 &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem) ページを参照してください。

>[!ENDTABS]

3 つの認証方法すべてが設定された、完全なサンプル `.env` ファイルを以下に示します。

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### サンプルアプリケーションの実行

サンプルアプリケーションでは、サンプルアセットをAEM as a Cloud Service環境にアップロードする 3 つの異なる方法を示します。

1. **FileSystemUpload** - ディレクトリ構造がサポートされ、自動フォルダー作成が可能なローカルファイルシステムからファイルをアップロードします
1. **DirectBinaryUpload** - [&#x200B; リモートファイル &#x200B;](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets) をアップロードします。 ファイルバイナリは、AEM as a Cloud Service環境にアップロードする前にメモリにバッファーされます。
1. **バッチアップロード** – 自動再試行ロジックとエラー回復を使用して、ローカルファイルシステムから複数のファイルをバッチでアップロードします。 そのバックグラウンドで、`FileSystemUpload` クラスを使用してローカルファイルシステムからファイルをアップロードします。

アップロードされるアセットは `sample-assets` フォルダーにあり、`img`、`video` および `doc` サブフォルダーを含みます。各サブフォルダーには、いくつかのサンプルアセットが含まれています。

1. サンプルアプリケーションを実行するには、次のコマンドを使用します。

```bash
$ npm start
```

1. 次の選択肢から目的のオプション _数値_ を入力します。

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

以下のタブに、各アップロード方法について、AEM as a Cloud Service環境でのサンプルアプリケーションの実行、出力およびアップロードされたアセットを示します。

>[!BEGINTABS]

>[!TAB FileSystemUpload]

1. `FileSystemUpload` のオプションのアプリケーション出力の例を次に示します。

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. AEM as a Cloud Service環境の `FileSystemUpload` オプションを使用してアップロードされたAssets:

   ![FileSystemUpload クラスを使用して、AEM as a Cloud Service環境にアセットをアップロードする &#x200B;](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB DirectBinaryUpload]

1. `DirectBinaryUpload` のオプションのアプリケーション出力の例を次に示します。

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. AEM as a Cloud Service環境の `DirectBinaryUpload` オプションを使用してアップロードされたAssets:

![DirectBinaryUpload クラスを使用して、AEM as a Cloud Service環境にアセットをアップロードする &#x200B;](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB  バッチアップロード ]

1. `Batch Upload` のオプションのアプリケーション出力の例を次に示します。

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. AEM as a Cloud Service環境の `Batch Upload` オプションを使用してアップロードされたAssets:

![BatchUpload クラスを使用してAEM as a Cloud Service環境にアセットをアップロードする &#x200B;](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## サンプルアプリケーションコードを確認します。

サンプルアプリケーションの主なエントリポイントは、`index.js` ファイルです。 `promptUser` 関数が含まれており、ユーザーに選択肢の選択を求め、選択した例を実行します。

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

完全なコードについては、サンプルアプリケーションの `index.js` ファイルを参照してください。

次のタブに、各アップロード方法の実装の詳細を示します。

>[!BEGINTABS]

>[!TAB FileSystemUpload]

`FileSystemUpload` クラスは、ディレクトリ構造のサポートおよび自動フォルダー作成を備えたローカルファイルシステムからファイルをアップロードするために使用されます。

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

完全なコードについては、サンプルアプリケーションの `examples/filesystem-upload.js` ファイルを参照してください。

>[!TAB DirectBinaryUpload]

`DirectBinaryUpload` クラスは、リモートファイルをAEM as a Cloud Service環境にアップロードするために使用されます。

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

完全なコードについては、サンプルアプリケーションの `examples/direct-binary-upload.js` ファイルを参照してください。

>[!TAB  バッチアップロード ]

ファイルをバッチに分割し、自動再試行ロジックとエラー回復を使用して、バッチでアップロードします。 そのバックグラウンドで、`FileSystemUpload` クラスを使用してローカルファイルシステムからファイルをアップロードします。

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

完全なコードについては、サンプルアプリケーションの `examples/batch-upload.js` ファイルを参照してください。

>[!ENDTABS]

また、サンプルアプリケーションの `README.md` ファイルには、サンプルアプリケーションの詳細なドキュメントが含まれています。

## ベストプラクティス

1. **適切な認証方法の選択：**
実稼動環境のサービス資格情報、開発/テスト専用のローカル開発トークンおよび基本認証を使用します。 サービス資格情報のユーザーが、AEM as a Cloud Service環境にアセットをアップロードするために必要な権限を持っていることを確認します。

1. **適切なアップロード方法を選択：**
自動フォルダー作成を使用するローカルファイルには FileSystemUpload を、きめ細かな制御を使用するストリーム/バッファー/リモート URL には DirectBinaryUpload を、再試行ロジックが必要な 1000 以上のファイルを含む実稼動環境にはバッチアップロードパターンを使用します。

1. **DirectBinaryUpload ファイル オブジェクトを正しく構造化する**
必須フィールド { fileName, fileSize, blob: buffer, targetFolder } を使用して blob プロパティ（バッファーではない）を使用します。DirectBinaryUpload では、フォルダーは自動作成されないことに注意してください。

1. **参照としてのサンプルアプリケーション：**
サンプルアプリケーションは、プログラムによるアセットのアップロードプロセスの実装の詳細を示す参考資料です。 独自の実装の出発点として使用できます。
