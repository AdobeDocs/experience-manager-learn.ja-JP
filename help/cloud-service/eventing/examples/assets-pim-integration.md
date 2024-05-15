---
title: PIM 統合用の AEM Assets イベント
description: アセットメタデータの更新に AEM Assets と製品情報管理（PIM）システムを統合する方法を説明します。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 761
last-substantial-update: 2024-02-13T00:00:00Z
jira: KT-14901
thumbnail: KT-14901.jpeg
exl-id: 070cbe54-2379-448b-bb7d-3756a60b65f0
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1116'
ht-degree: 100%

---

# PIM 統合用の AEM Assets イベント

>[!IMPORTANT]
>
>このチュートリアルでは、実験的な AEM as a Cloud Service API を使用します。これらの API へのアクセス権を取得するには、リリース前のソフトウェア使用許諾契約に同意し、アドビエンジニアリングがお使いの環境でこれらの API を手動で有効にする必要があります。アクセス権をリクエストするには、アドビサポートにお問い合わせください。

AEM Assets を製品情報管理（PIM）や製品ライン管理（PLM）システムなどのサードパーティシステムと統合して、**ネイティブ AEM IO イベントを使用**&#x200B;してアセットのメタデータを更新する方法を説明します。AEM Assets イベントを受け取ると、ビジネス要件に基づいて、AEM、PIM またはその両方のシステムでアセットメタデータを更新できます。ただし、この例では、AEM におけるアセットメタデータの更新方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3427592?quality=12&learn=on)

**AEM の外部**&#x200B;でアセットのメタデータ更新コードを実行するには、サーバーレスプラットフォームである [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/) を使用します。

イベント処理のフローは次のとおりです。

![PIM 統合用の AEM Assets イベント](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. AEM オーサーサービスは、アセットのアップロードが完了し、すべてのアセット処理アクティビティが完了すると、_アセット処理の完了_&#x200B;イベントをトリガーします。処理の完了を待つことで、標準の処理（メタデータの抽出など）が確実に完了します。
1. イベントは [Adobe I/Oイベント](https://developer.adobe.com/events/)サービスに送信されます。
1. Adobe I/O イベントサービスは、イベントを [Adobe I/O Runtime アクション](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)に渡して、処理します。
1. Adobe I/O Runtime アクションは、PIM システムの API を呼び出して、SKU、サプライヤー情報などの追加のメタデータを取得します。
1. PIM から取得した追加のメタデータは、AEM Assets で [Assets Author API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) を使用して更新されます。

## 前提条件

このチュートリアルを完了するには、次が必要になります。

- [AEM イベンティングが有効な](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) AEM as a Cloud Service 環境。また、サンプル [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) プロジェクトをそこにデプロイする必要があります。

- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/) へのアクセス権。

- ローカルマシンにインストールされた [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)。

## 開発手順

大まかな開発手順は次のとおりです。

1. [Adobe Developer Console（ADC）でプロジェクトの作成](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [プロジェクトをローカル開発用に初期化](./runtime-action.md#initialize-project-for-local-development)
1. ADC でプロジェクトの設定
1. ADC プロジェクトの通信を有効にする AEM オーサーサービスの設定
1. メタデータの取得と更新を編成するランタイムアクションの開発
1. アセットを AEM オーサーサービスにアップロードし、メタデータが更新されていることを確認

手順 1～2 について詳しくは、[Adobe I/O Runtime アクションと AEM イベント](./runtime-action.md#)の例を参照してください。順 3～6 について詳しくは、次の節を参照してください。

### Adobe Developer Console（ADC）でのプロジェクトの設定

AEM Assets イベントを受け取り、前の手順で作成した Adobe I/O Runtime アクションを実行するには、ADC でプロジェクトを設定します。

- ADC で、[プロジェクト](https://developer.adobe.com/console/projects)に移動します。`Stage` ワークスペース（ランタイムアクションがデプロイされた場所）を選択します。

- 「**サービスを追加**」ボタンをクリックし、「**イベント**」オプションを選択します。**イベントを追加**&#x200B;ダイアログで、**Experience Cloud**／**AEM Assets** を選択し、「**次へ**」をクリックします。追加の設定手順に従い、AEMCS インスタンス、_アセット処理の完了_&#x200B;イベント、OAuth サーバー間認証タイプおよびその他の詳細を選択します。

  ![AEM Assets イベント - イベントの追加](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- 最後に、**イベントの受信方法**&#x200B;の手順で、「**ランタイムアクション**」オプションを展開し、前のステップで作成した&#x200B;_汎用_&#x200B;アクションを選択します。「**設定済みイベントを保存**」をクリックします。

  ![AEM Assets イベント - イベントの受信](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- 同様に、「**サービスを追加**」ボタンをクリックし、「**API**」オプションを選択します。**API を追加**&#x200B;モーダルで、**Experience Cloud**／**AEM as a Cloud Service API** を選択し、「**次へ**」をクリックします。

  ![AEM as a Cloud Service API の追加 - プロジェクトの設定](../assets/examples/assets-pim-integration/add-aem-api.png)

- 次に、認証タイプとして **OAuth サーバー間**&#x200B;を選択し、「**次へ**」をクリックします。

- 次に、**AEM Administrators-XXX** 製品プロファイルを選択し、「**設定済み API を保存**」をクリックします。問題のアセットを更新するには、選択した製品プロファイルを、イベントの作成元である AEM Assets 環境に関連付け、そこでアセットを更新するための十分なアクセス権を持っている必要があります。

  ![AEM as a Cloud Service API の追加 - プロジェクトの設定](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

### AEM オーサーサービスを設定して、ADC プロジェクト通信を有効にする

上記の ADC プロジェクトから AEM のアセットメタデータを更新するには、ADC プロジェクトのクライアント ID を使用して AEM オーサーサービスを設定します。_クライアント ID_ は、[AdobeCloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=ja#add-variables) UI を使用して環境変数として追加されます。

- [AdobeCloud Manager](https://my.cloudmanager.adobe.com/) にログインし、**プログラム**／**環境**／**省略記号**／**詳細を表示**／「**設定**」タブを選択します。

  ![Adobe Cloud Manager - 環境の設定](../assets/examples/assets-pim-integration/cloud-manager-environment-configuration.png)

- 次に、「**設定を追加**」ボタンをクリックし、変数の詳細を次のように入力します。

  | 名前 | 値 | AEM サービス | タイプ |
  | ----------- | ----------- | ----------- | ----------- |
  | ADOBE_PROVIDED_CLIENT_ID | &lt;COPY_FROM_ADC_PROJECT_CREDENTIALS> | 作成者 | 変数 |

  ![Adobe Cloud Manager - 環境設定](../assets/examples/assets-pim-integration/add-environment-variable.png)

- 「**追加**」をクリックして、設定を「**保存**」します。

### ランタイムアクションの開発

メタデータの取得と更新を実行するには、まず `src/dx-excshell-1/actions/generic` フォルダーに自動作成された&#x200B;_汎用_&#x200B;アクションコードを更新します。

完全なコードについて詳しくは、添付の [WKND-Assets-PIM-Integration.zip](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip) ファイルを参照してください。以下の節では主要なファイルをハイライト表示しています。

- `src/dx-excshell-1/actions/generic/mockPIMCommunicator.js` ファイルは、PIM API 呼び出しをモックして、SKU やサプライヤー名などの追加のメタデータを取得します。このファイルはデモ用に使用されます。エンドツーエンドのフローが機能するようになったら、この関数を実際の PIM システムへの呼び出しに置き換えて、アセットのメタデータを取得します。

  ```javascript
  /**
   * Mock PIM API to get the product data such as SKU, Supplier, etc.
   *
   * In a real-world scenario, this function would call the PIM API to get the product data.
   * For this example, we are returning mock data.
   *
   * @param {string} assetId - The assetId to get the product data.
   */
  module.exports = {
      async getPIMData(assetId) {
          if (!assetId) {
          throw new Error('Invalid assetId');
          }
          // Mock response data for demo purposes
          const data = {
          SKUID: 'MockSKU 123',
          SupplierName: 'mock-supplier',
          // ... other product data
          };
          return data;
      },
  };
  ```

- `src/dx-excshell-1/actions/generic/aemCommunicator.js` ファイルは、[Assets Author API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) を使用して AEM のアセットメタデータを更新します。

  ```javascript
  const fetch = require('node-fetch');
  
  ...
  
  /**
  *  Get IMS Access Token using Client Credentials Flow
  *
  * @param {*} clientId - IMS Client ID from ADC project's OAuth Server-to-Server Integration
  * @param {*} clientSecret - IMS Client Secret from ADC project's OAuth Server-to-Server Integration
  * @param {*} scopes - IMS Meta Scopes from ADC project's OAuth Server-to-Server Integration as comma separated strings
  * @returns {string} - Returns the IMS Access Token
  */
  async function getIMSAccessToken(clientId, clientSecret, scopes) {
    const adobeIMSV3TokenEndpointURL = 'https://ims-na1.adobelogin.com/ims/token/v3';
  
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: `grant_type=client_credentials&client_id=${clientId}&client_secret=${clientSecret}&scope=${scopes}`,
    };
  
    const response = await fetch(adobeIMSV3TokenEndpointURL, options);
    const responseJSON = await response.json();
  
    return responseJSON.access_token;
  }    
  
  async function updateAEMAssetMetadata(metadataDetails, aemAssetEvent, params) {
    ...
    // Transform the metadata details to JSON Patch format,
    // see https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/#operation/patchAssetMetadata
    const transformedMetadata = Object.keys(metadataDetails).map((key) => ({
      op: 'add',
      path: `wknd-${key.toLowerCase()}`,
      value: metadataDetails[key],
    }));
  
    ...
  
    // Get ADC project's OAuth Server-to-Server Integration credentials
    const clientId = params.ADC_CECREDENTIALS_CLIENTID;
    const clientSecret = params.ADC_CECREDENTIALS_CLIENTSECRET;
    const scopes = params.ADC_CECREDENTIALS_METASCOPES;
  
    // Get IMS Access Token using Client Credentials Flow
    const access_token = await getIMSAccessToken(clientId, clientSecret, scopes);
  
    // Call AEM Author service to update the metadata using Assets Author API
    // See https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/
    const res = await fetch(`${aemAuthorHost}/adobe/assets/${assetId}/metadata`, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json-patch+json',
        'If-Match': '*',
        'X-Adobe-Accept-Experimental': '1',
        'X-Api-Key': 'aem-assets-management-api', // temporary value
        Authorization: `Bearer ${access_token}`,
      },
      body: JSON.stringify(transformedMetadata),
    });
  
    ...
  }
  
  module.exports = { updateAEMAssetMetadata };
  ```

  `.env` ファイルには、ADC プロジェクトの OAuth サーバー間認証情報の詳細が格納され、これらは `ext.config.yaml` ファイルを使用してアクションにパラメーターとして渡されます。シークレットとアクションパラメーターの管理について詳しくは、[App Builder 設定ファイル](https://developer.adobe.com/app-builder/docs/guides/configuration/)を参照してください。

- `src/dx-excshell-1/actions/model` フォルダーには、`aemAssetEvent.js` および `errors.js` ファイルが含まれています。これらのファイルは、アクションが受け取ったイベントを解析し、エラーを処理するために使用されます。

- `src/dx-excshell-1/actions/generic/index.js` ファイルでは、前述のモジュールを使用して、メタデータの取得と更新を調整します。

  ```javascript
  ...
  
  let responseMsg;
  // handle the challenge probe request, they are sent by I/O to verify the action is valid
  if (params.challenge) {
    logger.info('Challenge probe request detected');
    responseMsg = JSON.stringify({ challenge: params.challenge });
  } else {
    logger.info('AEM Asset Event request received');
  
    // create AEM Asset Event object from request parameters
    const aemAssetEvent = new AEMAssetEvent(params);
  
    // Call mock PIM API to get the product data such as SKU, Supplier, etc.
    const mockPIMData = await mockPIMAPI.getPIMData(
      aemAssetEvent.getAssetName(),
    );
    logger.info('Mock PIM API response', mockPIMData);
  
    // Update PIM received data in AEM as Asset metadata
    const aemUpdateStatus = await updateAEMAssetMetadata(
      mockPIMData,
      aemAssetEvent,
      params,
    );
    logger.info('AEM Asset metadata update status', aemUpdateStatus);
  
    if (aemUpdateStatus) {
      // create response message
      responseMsg = JSON.stringify({
        message:
          'AEM Asset Event processed successfully, updated the asset metadata with PIM data.',
        assetdata: {
          assetName: aemAssetEvent.getAssetName(),
          assetPath: aemAssetEvent.getAssetPath(),
          assetId: aemAssetEvent.getAssetId(),
          aemHost: aemAssetEvent.getAEMHost(),
          pimdata: mockPIMData,
        },
      });
    } 
  
    // response object
    const response = {
      statusCode: 200,
      body: responseMsg,
    };
  
    // Return the response to the caller
    return response;
  
    ...
  }
  ```

次のコマンドを使用して、更新したアクションを Adobe I/O Runtime にデプロイします。

```bash
$ aio app deploy
```

### アセットのアップロードとメタデータの検証

AEM Assets と PIM の統合を検証するには、次の手順に従います。

- モック PIM で提供された SKU やサプライヤー名などのメタデータを表示するには、AEM Assets でメタデータスキーマを作成します。SKU とサプライヤー名のメタデータプロパティを表示する[メタデータスキーマ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/configuring/metadata-schemas.html?lang=ja)を参照してください。

- AEM オーサーサービスでアセットをアップロードし、メタデータの更新を検証します。

  ![AEM Assets メタデータの更新](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## 概念と重要な留意点

企業では、AEM と PIM などの他のシステムとの間でアセットメタデータを同期する必要が生じる場合が多くあります。AEM イベンティングを使用すると、このような要件を満たすことができます。

- アセットのメタデータ取得コードは AEM の外部で実行されるので、AEM オーサーサービスの負荷が回避され、独立して拡張できるイベント駆動型のアーキテクチャになります。
- 新しく導入された Assets Author API は、AEM のアセットメタデータの更新に使用されます。
- API 認証では、OAuth サーバー間（別名クライアント資格情報フロー）を使用します。[OAuth サーバー間の資格情報実装ガイド](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/)を参照してください。
- Adobe I/O Runtime アクションの代わりに、他の web フックまたは Amazon EventBridge を使用して AEM Assets イベントを受け取り、メタデータの更新を処理できます。
- AEM イベンティングを通じたアセットイベントにより、企業が重要なプロセスを自動化および合理化し、コンテンツエコシステム全体の効率と一貫性を促進できます。
