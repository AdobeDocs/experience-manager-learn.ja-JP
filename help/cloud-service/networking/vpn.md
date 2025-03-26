---
title: 仮想プライベートネットワーク（VPN）
description: AEM as a Cloud Service を VPN に接続して、AEM と内部サービスの間に安全な通信チャネルを作成する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
last-substantial-update: 2024-04-27T00:00:00Z
duration: 919
source-git-commit: e1bea4320ed7a8b6d45f674649ba9ba946054b17
workflow-type: tm+mt
source-wordcount: '1556'
ht-degree: 82%

---

# 仮想プライベートネットワーク（VPN）

AEM as a Cloud Service を VPN に接続して、AEM と内部サービスの間に安全な通信チャネルを作成する方法を説明します。

>[!IMPORTANT]
>
>VPN とポート転送は、Cloud Manager UI を通じて、または API 呼び出しを使用して設定できます。 このチュートリアルでは、API メソッドに焦点を当てます。
>
>UI を使用する場合は、[AEM as a Cloud Serviceの高度なネットワーク機能の設定 ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) を参照してください。

## 仮想プライベートネットワークとは

仮想プライベートネットワーク（VPN）により、AEM as a Cloud Service のお客様は、Cloud Manager プログラム内の **AEM 環境**&#x200B;を、[サポートされている](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)既存の VPN に接続できます。 VPN により、AEM as a Cloud Service とお客様のネットワーク内のサービスとの間で、安全で制御された接続を実現できます。

Cloud Manager プログラムでは、__単一の__&#x200B;ネットワークインフラストラクチャタイプのみを持つことができます。 次のコマンドを実行する前に、仮想プライベートネットワークが AEM as a Cloud Service に最も[適切なタイプのネットワークインフラストラクチャ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)であることを確認してください。

>[!NOTE]
>
>ビルド環境を Cloud Manager から VPN に接続することはサポートされていません。 プライベートリポジトリからバイナリアーティファクトにアクセスする必要がある場合は、[ここで説明するように](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project)、パブリックインターネット上からアクセスできる URL を使用して、パスワードで保護されたセキュアなリポジトリを設定する必要があります。

>[!MORELIKETHIS]
>
> 仮想プライベートネットワークについて詳しくは、AEM as a Cloud Service の[詳細なネットワーク設定ドキュメント](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)を参照してください。

## 前提条件

Cloud Manager API を使用して仮想プライベートネットワークを設定する場合は、次が必要です。

+ [Cloud Manager のビジネスオーナー権限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)のある Adobeアカウント
+ [Cloud Manager API の認証資格情報](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)へのアクセス
   + 組織 ID（別名 IMS 組織 ID）
   + クライアント ID（API キー）
   + アクセストークン（Bearer トークン）
+ Cloud Manager プログラム ID
+ Cloud Manager 環境 ID
+ 必要なすべての接続パラメーターにアクセスできる&#x200B;**ルートに戻づいた**&#x200B;仮想プライベートネットワーク。

詳しくは [Cloud Manger API 資格情報の設定、設定および取得方法を確認して ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/developing/extensibility/app-builder/server-to-server-auth)Cloud Manager API 呼び出しに使用します。

>[!IMPORTANT]
>
>このチュートリアルでは、`curl` を使用してCloud Manager API を設定します *プログラムによるアプローチを希望する場合*。 指定された `curl` コマンドは、Linux® またはmacOS構文を想定しています。 Windows のコマンドプロンプトを使用する場合は、`\` 改行文字を `^` で置換します。
>
>または、Cloud Manager UI を使用して同じタスクを実行することもできます。 *UI アプローチをご希望の場合*、[AEM as a Cloud Serviceの高度なネットワーク機能の設定 ](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking) を参照してください。

## プログラムごとの仮想プライベートネットワークの有効化

AEM as a Cloud Service の仮想プライベートネットワークを有効化することから始めます。


>[!BEGINTABS]

>[!TAB Cloud Manager]

Cloud Manager を使用すると、フレキシブルポートエグレスを有効にすることができます。次の手順では、Cloud Manager を使用して AEM as a Cloud Service でフレキシブルポートエグレスを有効にする方法の概要について説明します。

1. Cloud Manager ビジネスオーナーとして [Adobe Experience Manager Cloud Manager](https://experience.adobe.com/cloud-manager/) にログインします。
1. 目的のプログラムに移動します。
1. 左側のメニューで、__サービス／ネットワークインフラストラクチャ__&#x200B;に移動します。
1. 「__ネットワークインフラストラクチャを追加__」ボタンを選択します。

   ![ネットワークインフラストラクチャを追加](./assets/cloud-manager__add-network-infrastructure.png)

1. __ネットワークインフラストラクチャを追加__ ダイアログボックスで、「__仮想プライベートネットワーク__」オプションを選択します。 フィールドに入力して、「__続行__」を選択します。組織のネットワーク管理者と連携して、正しい値を取得します。

   ![VPN を追加](./assets/vpn/select-type.png)

1. 1 つ以上の VPN 接続を作成します。接続に意味のある名前を付けて、「__接続を追加__」ボタンを選択します。

   ![VPN 接続を追加](./assets/vpn/add-connection.png)

1. VPN 接続を設定します。組織のネットワーク管理者と連携して、正しい値を取得します。「__保存__」を選択して、接続の追加を確認します。

   ![VPN 接続を設定](./assets/vpn/configure-connection.png)

1. 複数の VPN 接続が必要な場合は、必要に応じてさらに接続します。すべての VPN 接続を追加する際は、「__続行__」を選択します。

   ![VPN 接続を設定](./assets/vpn/connections.png)

1. 「__保存__」を選択して、VPN と設定済みのすべての接続の追加を確認します。

   ![VPN 作成を確認](./assets/vpn/confirmation.png)

1. ネットワークインフラストラクチャが作成され、__準備完了__&#x200B;とマークされるまで待ちます。このプロセスには最大 1 時間かかる場合があります。

   ![VPN の作成ステータス](./assets/vpn/creating.png)

VPN を作成したので、以下の説明に従って Cloud Manager API を使用して VPN を設定できます。

>[!TAB Cloud Manager API]

Cloud Manager API を使用して、仮想プライベートネットワークを有効にすることができます。次の手順では、Cloud Manager API を使用して AEM as a Cloud Service で VPN を有効にする方法の概要について説明します。

1. まず、Cloud Manager API の [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して、詳細なネットワーク構成が必要な地域を決定します。 `region name` は後続の Cloud Manager API 呼び出しを行うために必要です。 通常、実稼動環境が存在する地域が使用されます。

   [Cloud Manager](https://my.cloudmanager.adobe.com) で、[環境の詳細](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments)の下にある AEM as a Cloud Service 環境の地域を見つけます。 Cloud Manager に表示される地域名は、Cloud Manager API で使用される[地域コードにマッピング](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments?lang=ja)できます。

   __listRegions HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Cloud Manager API の [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して、Cloud Manager プログラムの仮想プライベートネットワークを有効化します。 Cloud Manager API の `listRegions` 操作から取得した適切な `region` コードを使用します。

   __createNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   `vpn-create.json` で JSON パラメーターを定義し、`... -d @./vpn-create.json` を介して curl に提供します。

   [サンプルの vpn-create.json をダウンロードします](./assets/vpn-create.json)。  このファイルは例に過ぎません。[enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) に記載されているオプション／必須フィールドに基づいて、必要に応じてファイルを設定します。

   ```json
   {
       "kind": "vpn",
       "region": "va7",
       "addressSpace": [
           "10.104.182.64/26"
       ],
       "dns": {
           "resolvers": [
               "10.151.201.22",
               "10.151.202.22",
               "10.154.155.22"
           ],
           "domains": [
               "wknd.site",
               "wknd.com"
           ]
       },
       "connections": [{
           "name": "connection-1",
           "gateway": {
               "address": "195.231.212.78",
               "addressSpace": [
                   "10.151.0.0/16",
                   "10.152.0.0/16",
                   "10.153.0.0/16",
                   "10.154.0.0/16",
                   "10.142.0.0/16",
                   "10.143.0.0/16",
                   "10.124.128.0/17"
               ]
           },
           "sharedKey": "<secret_shared_key>",
           "ipsecPolicy": {
               "dhGroup": "ECP256",
               "ikeEncryption": "AES256",
               "ikeIntegrity": "SHA256",
               "ipsecEncryption": "AES256",
               "ipsecIntegrity": "SHA256",
               "pfsGroup": "ECP256",
               "saDatasize": 102400000,
               "saLifetime": 3600
           }
       }]
   }
   ```

   Cloud Manager プログラムがネットワークインフラストラクチャをプロビジョニングするまで 45 ～ 60 分待ちます。

1. 環境が、Cloud Manager API の [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作を使用して&#x200B;__仮想プライベートネットワーク__&#x200B;の設定を完了したことを、前の手順で `createNetworkInfrastructure` HTTP リクエストから返された `id` を使用して確認します。

   __getNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 応答に、__準備完了__&#x200B;の&#x200B;__ステータス__&#x200B;が含まれていることを確認します。 まだ準備完了ではない場合は、数分ごとにステータスを再確認します。


VPN を作成したので、以下の説明に従って Cloud Manager API を使用して VPN を設定できます。

>[!ENDTABS]

## 環境ごとの仮想プライベートネットワークプロキシの設定

1. Cloud Manager API の [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して、各 AEM as a Cloud Service 環境で&#x200B;__仮想プライベートネットワーク__&#x200B;の設定を作成します。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   `vpn-configure.json` で JSON パラメーターを定義し、`... -d @./vpn-configure.json` を介して curl に提供します。

[サンプル vpn-configure.json のダウンロード](./assets/vpn-configure.json)

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org"
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.com",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   `nonProxyHosts` は、ポート 80 または 443 が専用のエグレス IP ではなくデフォルトの共有 IP アドレス範囲を介してルーティングされるホストのセットを宣言します。 `nonProxyHosts` は、Adobeが自動的に最適化する共有 IP を介してトラフィックが送信される場合に役立ちます。

   各 `portForwards` マッピングでは、詳細ネットワークは次の転送ルールを定義します。

   | プロキシホスト | プロキシポート |  | 外部ホスト | 外部ポート |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEM デプロイメントで必要とする外部サービスが HTTP／HTTPS __のみ__&#x200B;の場合は、`portForwards` 配列を空のままにします。これらのルールは非 HTTP／HTTPS リクエストにのみ必要なためです。


2. 環境ごとに、Cloud Manager API の [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して、VPN ルーティングルールが有効であることを検証します。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

3. 仮想プライベートネットワークのプロキシ設定は、Cloud Manager API の [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して更新できます。 `enableEnvironmentAdvancedNetworkingConfiguration` は `PUT` 操作であるため、この操作を呼び出すたびにすべてのルールを提供する必要があることに注意してください。

4. これで、カスタム AEM コードおよび設定で仮想プライベートネットワークエグレス設定を使用できるようになりました。

## 仮想プライベートネットワーク経由での外部サービスへの接続

仮想プライベートネットワークを有効にすると、AEMのコードと設定で使用して、VPN 経由で外部サービスを呼び出すことができます。 外部呼び出しには、AEM での処理方法が 2 種類あります。

1. 外部サービスへの HTTP／HTTPS 呼び出し
   + これらの外部サービスには、標準ポート 80 または 443 以外のポートで動作するサービスに対して行われる HTTP/HTTPS 呼び出しが含まれます。
1. 外部サービスへの HTTP/HTTPS 以外の呼び出し
   + これらの外部サービスには、メールサーバー、SQL データベース、HTTP/HTTPS 以外のプロトコルを使用するサービスへの接続など、HTTP 以外の呼び出しが含まれます。

AEM からの標準ポート（80／443）での HTTP／HTTPS リクエストはデフォルトで許可されていますが、以下のように適切に設定されていない場合、VPN 接続は使用されません。

### HTTP／HTTPS

AEM から HTTP／HTTPS で接続する場合、VPN を使用すると、HTTP／HTTPS 接続が自動的 AEM からプロキシされます。 HTTP／HTTPS 接続をサポートするために、追加のコードや設定は必要ありません。

>[!TIP]
>
> [ルーティングルールの完全なセット](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking)については、AEM as a Cloud Service の仮想プライベートネットワークのドキュメントを参照してください。

#### コードの例

<table>
<tr>
<td>
    <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP／HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP／HTTPS</a></strong></div>
    <p>
        HTTP／HTTPS プロトコルを使用して、AEM as a Cloud Service から外部サービスへの HTTP／HTTPS 接続を作成する Java™ コードの例。
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### 非 HTTP／HTTPS 接続コードの例

AEM から HTTP／HTTPS 以外の接続を作成する場合（例：SQL、SMTP など）、接続は AEM から提供される特別なホスト名を使用して行う必要があります。

| 変数名 | 使用方法 | Java™ コード | OSGi 設定 |
| - |  - | - | - |
| `AEM_PROXY_HOST` | HTTP／HTTPS 以外の接続用のプロキシホスト | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


外部サービスへの接続は、その後、`AEM_PROXY_HOST` とマッピングされたポート（`portForwards.portOrig`）を使用しで呼び出されます。次に、AEM はこれをマッピングされた外部ホスト名（`portForwards.name`）とポート（`portForwards.portDest`）にルーティングします。

| プロキシホスト | プロキシポート |  | 外部ホスト | 外部ポート |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |


#### コードの例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="JDBC DataSourcePool を使用した SQL 接続" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">JDBC DataSourcePool を使用した SQL 接続</a></strong></div>
      <p>
            AEM JDBC データソースプールを設定して外部 SQL データベースに接続する Java™コード例。
      </p>
    </td>
   <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Java API を使用した SQL 接続" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Java™ API を使用した SQL 接続</a></strong></div>
      <p>
            Java™ の SQL API を使用して外部 SQL データベースに接続する Java™ コードの例。
      </p>
    </td>
   <td>
      <a  href="./examples/email-service.md"><img alt="仮想プライベートネットワーク（VPN）" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">メールサービス</a></strong></div>
      <p>
        外部のメールサービスへの接続に AEM を使用する OSGi 設定例です。
      </p>
    </td>
</tr></table>

### VPN を使用してAEM as a Cloud Serviceへのアクセスを制限する

仮想プライベートネットワークの設定で、AEM as a Cloud Service 環境へのアクセスを VPN に制限します。

#### 設定例

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list"><img alt="IP 許可リストの適用" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list">IP 許可リストの適用</a></strong></div>
      <p>
            VPN トラフィックのみが AEM にアクセスできるように IP 許可リストを設定します。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking"><img alt="AEM パブリッシュへのパスベースの VPN アクセス制限" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking">AEM パブリッシュへのパスベースの VPN アクセス制限</a></strong></div>
      <p>
            AEM パブリッシュ上の特定のパスに対して VPN アクセスを要求します。
      </p>
    </td>
   <td></td>
</tr></table>
