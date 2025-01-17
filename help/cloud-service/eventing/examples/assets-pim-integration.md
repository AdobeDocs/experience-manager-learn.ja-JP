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
source-git-commit: 99aa43460a76460175123a5bfe5138767491252b
workflow-type: tm+mt
source-wordcount: '1489'
ht-degree: 31%

---

# PIM 統合用の AEM Assets イベント

>[!IMPORTANT]
>
>このチュートリアルでは、OpenAPI ベースのAEM API を使用します。 早期アクセスプログラムの一部としてご利用いただけます。早期アクセスプログラムへのアクセスに興味がある場合は、ユースケースの説明を電子メール ](mailto:aem-apis@adobe.com)0}aem-apis@adobe.com} でお知らせください。[

OpenAPI ベースのAssets オーサー API を使用して、AEM イベントを受け取り、それに基づいてAEMのコンテンツの状態を更新する方法を説明します。

受信したイベントの処理方法は、ビジネス要件によって異なります。 例えば、イベントデータを使用して、サードパーティシステム、AEM、またはその両方を更新できます。

この例では、Product Information Management （PIM）システムなどのサードパーティシステムをAEM as a Cloud Service Assetsと統合する方法を示しています。 AEM Assets イベントを受け取ると、追加のメタデータを PIM システムから取得してAEMで更新するために処理されます。 更新されたアセットメタデータには、SKU、サプライヤー名、その他の製品詳細などの追加情報を含めることができます。

AEM Assets イベントを受け取って処理するには、サーバーレスのプラットフォームである ](https://developer.adobe.com/runtime/docs/guides/overview/what_is_runtime/)0}Adobe I/O Runtime} が使用されます。 [ただし、サードパーティシステムの Webhook やAmazon EventBridge など、他のイベント処理システムも使用できます。

統合の大まかなフローは次のとおりです。

![PIM 統合用の AEM Assets イベント](../assets/examples/assets-pim-integration/aem-assets-pim-integration.png)

1. AEM オーサーサービスは、アセットのアップロードが完了し、すべてのアセット処理アクティビティも完了した場合に _アセット処理完了_ イベントをトリガーします。 アセット処理の完了を待つことで、メタデータ抽出などの標準処理が確実に完了します。
1. イベントは [Adobe I/Oイベント](https://developer.adobe.com/events/)サービスに送信されます。
1. Adobe I/O イベントサービスは、イベントを [Adobe I/O Runtime アクション](https://developer.adobe.com/runtime/docs/guides/using/creating_actions/)に渡して、処理します。
1. Adobe I/O Runtime アクションは、PIM システムの API を呼び出して、SKU、サプライヤー情報などの追加のメタデータを取得します。
1. PIM から取得された追加のメタデータは、OpenAPI ベースの [AEM Assets オーサー API](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/assets/author/) を使用してAssetsで更新されます。

## 前提条件

このチュートリアルを完了するには、次が必要になります。

- [AEM イベンティングが有効な](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment) AEM as a Cloud Service 環境。また、サンプル [WKND Sites](https://github.com/adobe/aem-guides-wknd?#aem-wknd-sites-project) プロジェクトをそこにデプロイする必要があります。

- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/getting-started/) にアクセスします。

- ローカルマシンにインストールされた [Adobe Developer CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)。

## 開発手順

大まかな開発手順は次のとおりです。

1. [AEM as a Cloud Service環境の近代化 ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis#modernization-of-aem-as-a-cloud-service-environment)
1. [AEM API アクセスの有効化 ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis#enable-aem-apis-access)
1. [Adobe Developer Console（ADC）でプロジェクトの作成](./runtime-action.md#Create-project-in-Adobe-Developer-Console)
1. [プロジェクトをローカル開発用に初期化](./runtime-action.md#initialize-project-for-local-development)
1. ADC でプロジェクトの設定
1. ADC プロジェクトの通信を有効にする AEM オーサーサービスの設定
1. 調整するランタイムアクションの開発
   1. pim システムからのメタデータの取得
   1. AEM Assets オーサー API を使用したAssetsでのメタデータの更新
1. アセットメタデータスキーマの作成と適用
1. アセットのアップロードとメタデータの更新の検証

手順 1～2 について詳しくは [OpenAPI ベースのAEM API の呼び出し ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis) ガイドを参照し、手順 3～4 について [Adobe I/O RuntimeのアクションとAEM イベント ](./runtime-action.md#) の例を参照してください。 手順 5～9 については、次の節を参照してください。

### Adobe Developer Console（ADC）でのプロジェクトの設定

AEM Assets イベントを受け取り、前の手順で作成した Adobe I/O Runtime アクションを実行するには、ADC でプロジェクトを設定します。

- ADC で、手順 3 で作成した [ プロジェクト ](https://developer.adobe.com/console/projects) に移動します。 そのプロジェクトから、手順 4 の手順の一部として `aio app deploy` を実行するとランタイムアクションがデプロイされる `Stage` ワークスペースを選択します。

- 「**サービスを追加**」ボタンをクリックし、「**イベント**」オプションを選択します。**イベントを追加** ダイアログで、「**Experience Cloud**」/「**AEM Assets**」を選択し、「**次へ**」をクリックします。
  ![AEM Assets イベント - イベントの追加](../assets/examples/assets-pim-integration/add-aem-assets-event.png)

- **イベント登録の設定** 手順で、目的の AEMCS インスタンス、「_アセット処理が完了しました_」イベント、「OAuth サーバー間認証タイプ」の順に選択します。

  ![AEM Assets イベント – イベントの設定 ](../assets/examples/assets-pim-integration/configure-aem-assets-event.png)

- 最後に、「**イベントの受信方法**」手順で「**ランタイムアクション**」オプションを展開し、前の手順で作成した _汎用_ アクションを選択します。 「**設定済みイベントを保存**」をクリックします。

  ![AEM Assets イベント - イベントの受信](../assets/examples/assets-pim-integration/receive-aem-assets-event.png)

- 同様に、「**サービスを追加**」ボタンをクリックし、「**API**」オプションを選択します。**API を追加** モーダルで、「**Experience Cloud**」/「**AEM Assets オーサー API**」を選択し、「**次へ**」をクリックします。

  ![AEM Assets オーサー API を追加 – プロジェクトを設定 ](../assets/examples/assets-pim-integration/add-aem-api.png)

- 次に、認証タイプとして **OAuth サーバー間**&#x200B;を選択し、「**次へ**」をクリックします。

- 次に、イベントを生成するAEM Assets環境に関連付けられている正しい **製品プロファイル** を選択し、そこでアセットを更新するために十分なアクセス権を持ちます。 最後に、「**設定済み API を保存**」ボタンをクリックします。

  ![AEM Assets オーサー API を追加 – プロジェクトを設定 ](../assets/examples/assets-pim-integration/add-aem-api-product-profile-select.png)

  この場合、_AEM Administrators - author - Program XXX - Environment YYY_ 製品プロファイルが選択され、**AEM Assets API Users** サービスが有効になっています。

  ![AEM Assets オーサー API の追加 – 製品プロファイルの例 ](../assets/examples/assets-pim-integration/aem-api-product-profile.png)

## AEM インスタンスを設定して、ADC プロジェクト通信を有効にします

ADC プロジェクトの OAuth サーバー間資格情報クライアント ID をAEM インスタンスと通信できるようにするには、AEM インスタンスを設定する必要があります。

それには、AEM プロジェクトの `config.yaml` ファイルで設定を定義します。 次に、Cloud Managerで設定パイプラインを使用して `config.yaml` ファイルをデプロイします。

- AEM プロジェクトで、`config` フォルダーから `config.yaml` ファイルを探すか作成します。

  ![ 設定 YAML を見つける ](../assets/examples/assets-pim-integration/locate-config-yaml.png)

- 次の設定を `config.yaml` ファイルに追加します。

  ```yaml
  kind: "API"
  version: "1.0"
  metadata: 
      envTypes: ["dev", "stage", "prod"]
  data:
      allowedClientIDs:
          author:
          - "<ADC Project's OAuth Server-to-Server credential ClientID>"
  ```

  `<ADC Project's OAuth Server-to-Server credential ClientID>` を ADC プロジェクトの OAuth サーバー間資格情報の実際の ClientID に置き換えます。

- 設定の変更を Git リポジトリにコミットし、変更内容をリモートリポジトリにプッシュします。

- Cloud Managerで設定パイプラインを使用して、上記の変更をデプロイします。 コマンドラインツールを使用して `config.yaml` ファイルを RDE にインストールすることもできます。

  ![config.yaml のデプロイ ](../assets/examples/assets-pim-integration/config-pipeline.png)

### ランタイムアクションの開発

メタデータの取得と更新を実行するには、まず `src/dx-excshell-1/actions/generic` フォルダーに自動作成された&#x200B;_汎用_&#x200B;アクションコードを更新します。

コード全体については、添付の [WKND-Assets-PIM-Integration.zip](../assets/examples/assets-pim-integration/WKND-Assets-PIM-Integration.zip) ファイルを参照してください。以下の節では、主要なファイルについて説明します。

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

  `.env` ファイルには、ADC プロジェクトの OAuth サーバー間資格情報の詳細が格納され、それらをパラメーターとして、ファイルを使用してアクションに渡 `ext.config.yaml` ます。 シークレットとアクションパラメーターの管理について詳しくは、[App Builder 設定ファイル](https://developer.adobe.com/app-builder/docs/guides/configuration/)を参照してください。
- `src/dx-excshell-1/actions/model` フォルダーには、`aemAssetEvent.js` および `errors.js` ファイルが含まれています。これらのファイルは、アクションが受け取ったイベントを解析し、エラーを処理するために使用されます。
- `src/dx-excshell-1/actions/generic/index.js` ファイルでは、上記のモジュールを使用して、メタデータの取得と更新を調整します。

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

- 次のコマンドを使用して、更新したアクションを Adobe I/O Runtime にデプロイします。

  ```bash
  $ aio app deploy
  ```

### アセットメタデータスキーマの作成と適用

デフォルトでは、WKND Sites プロジェクトには、SKU、サプライヤー名など PIM 固有のメタデータを表示するためのアセットメタデータスキーマがありません。 アセットメタデータスキーマを作成して、AEM インスタンスのアセットフォルダーに適用します。

1. AEM as a Cloud Service Asset インスタンスにログインし、[ アセットビュー ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/authoring/switch-views) を開きます。

   ![AEM Assets ビュー ](../assets/examples/assets-pim-integration/aem-assets-view.png)

1. 左側のパネルから **設定**/**メタデータForms** オプションに移動し、「**作成**」ボタンをクリックします。 **メタデータフォームを作成** ダイアログで、次の詳細を入力し、「**作成**」をクリックします。
   - 名前：`PIM`
   - 既存のフォーム構造をテンプレートとして使用：`Check`
   - 次の中から選択します。`default`

   ![ メタデータフォームの作成 ](../assets/examples/assets-pim-integration/create-metadata-form.png)

1. **+** アイコンをクリックして新しい **PIM** タブを追加し、そのタブに **1 行のテキスト** コンポーネントを追加します。

   ![ 「PIM を追加」タブ ](../assets/examples/assets-pim-integration/add-pim-tab.png)

   次の表に、メタデータのプロパティと、それに対応するフィールドを示します。

   | ラベル | Placeholder | メタデータプロパティ |
   | --- | --- | --- |
   | SKU | AEM イベント統合による自動入力 | `wknd-skuid` |
   | サプライヤー名 | AEM イベント統合による自動入力 | `wknd-suppliername` |

1. **保存** および **閉じる** をクリックして、メタデータフォームを保存します。

1. 最後に、**PIM** メタデータスキーマを **PIM** フォルダーに適用します。

   ![ メタデータスキーマの適用 ](../assets/examples/assets-pim-integration/apply-metadata-schema.png)

上記の手順で、**Adventures** フォルダーのアセットは、SKU、サプライヤー名などの PIM 固有のメタデータを表示する準備が整いました。

### アセットのアップロードとメタデータの検証

AEM Assetsと PIM の統合を確認するには、AEM Assetsの **Adventures** フォルダーにアセットをアップロードします。 アセットの詳細ページの「PIM」タブには、SKU とサプライヤー名のメタデータが表示されます。

![AEM Assetsのアップロード ](../assets/examples/assets-pim-integration/aem-assets-metadata-update.png)

## 概念と重要な留意点

企業では、AEM と PIM などの他のシステムとの間でアセットメタデータを同期する必要が生じる場合が多くあります。AEM イベンティングを使用すると、このような要件を満たすことができます。

- アセットメタデータ取得コードはAEMの外部で実行されるので、AEM オーサーサービスの負荷が回避されます。したがって、独立して拡張されるイベント駆動型のアーキテクチャになります。
- 新しく導入された Assets Author API は、AEM のアセットメタデータの更新に使用されます。
- API 認証では、OAuth サーバー間（別名クライアント資格情報フロー）を使用します。[OAuth サーバー間の資格情報実装ガイド](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/)を参照してください。
- Adobe I/O Runtime アクションの代わりに、他の web フックまたは Amazon EventBridge を使用して AEM Assets イベントを受け取り、メタデータの更新を処理できます。
- AEMイベントを通じたアセットイベント イベントは、重要なプロセスを自動化および合理化し、コンテンツエコシステム全体で効率と一貫性を促進する機能を企業に提供します。
