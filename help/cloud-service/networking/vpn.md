---
title: 仮想プライベートネットワーク（VPN）
description: AEMと VPN をas a Cloud Serviceに接続して、AEMと内部サービスとの間に安全な通信チャネルを作成する方法を説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9352
thumbnail: KT-9352.jpeg
exl-id: 74cca740-bf5e-4cbd-9660-b0579301a3b4
source-git-commit: 52a2303f75c23c72e201b1f674f7f882db00710b
workflow-type: tm+mt
source-wordcount: '1364'
ht-degree: 1%

---

# 仮想プライベートネットワーク（VPN）

AEMと VPN をas a Cloud Serviceに接続して、AEMと内部サービスとの間に安全な通信チャネルを作成する方法を説明します。

## 仮想プライベートネットワークとは

仮想プライベートネットワーク (VPN) を使用すると、AEMas a Cloud Serviceのお客様が接続できます **AEM環境** を既存の [サポート](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) VPN。 これにより、お客様のネットワーク内のAEMas a Cloud Serviceのサービスとサービス間の安全で制御された接続が可能になります。

Cloud Manager プログラムでは、 __シングル__ ネットワークインフラストラクチャのタイプ。 仮想プライベートネットワークが最も多いことを確認する [適切な種類のネットワークインフラストラクチャ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#general-vpn-considerations) 次のコマンドを実行する前にAEMas a Cloud Serviceのに対して実行します。

>[!NOTE]
>
>ビルド環境を Cloud Manager から VPN に接続することはサポートされていません。 プライベートリポジトリからバイナリアーティファクトにアクセスする必要がある場合は、パブリックインターネット上で使用できる URL を使用して、セキュリティで保護されたパスワードで保護されたリポジトリを設定する必要があります [ここで説明するように](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/create-application-project/setting-up-project.html#password-protected-maven-repositories).

>[!MORELIKETHIS]
>
> AEMを読む [高度なネットワーク設定ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn) 仮想プライベートネットワークの詳細については、を参照してください。

## 前提条件

仮想プライベートネットワークを設定する場合は、次の操作が必要です。

+ Adobeアカウント [Cloud Manager のビジネスオーナー権限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ アクセス先 [Cloud Manager API の認証資格情報](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織 ID（別名 IMS Org ID）
   + クライアント ID（別名 API キー）
   + アクセストークン（Bearer トークン）
+ Cloud Manager プログラム ID
+ Cloud Manager 環境 ID
+ 必要なすべての接続パラメーターにアクセスできる仮想プライベートネットワーク。

詳しくは、次の Cloud Manager API 資格情報の設定、設定、取得方法、およびそれらを使用した Cloud Manager API 呼び出しの作成方法に関するチュートリアルを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

このチュートリアルでは、 `curl` Cloud Manager API を設定する場合。 指定された `curl` コマンドは、Linux/macOS構文を想定しています。 Windows のコマンドプロンプトを使用する場合は、 `\` 改行文字 `^`.

## プログラムごとの仮想プライベートネットワークの有効化

まず、AEMの仮想プライベートネットワークを有効にします。

1. まず、Cloud Manager API を使用してアドバンスドネットワークが設定される地域を決定します [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 この `region name` 後続の Cloud Manager API 呼び出しをおこなうために必要になります。 通常、実稼動環境が存在する地域が使用されます。

   __listRegions HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. Cloud Manager API を使用した Cloud Manager プログラムの仮想プライベートネットワークの有効化 [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 適切な `region` Cloud Manager API から取得したコード `listRegions` 操作。

   __createNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
       -d @./vpn-create.json
   ```

   JSON パラメーターを `vpn-create.json` を介してカールするために提供されます。 `... -d @./vpn-create.json`.

   [vpn-create.json の例をダウンロードします。](./assets/vpn-create.json).  このファイルは例に過ぎません。 次のドキュメントに記載されているオプション/必須フィールドに基づいて、必要に応じてファイルを設定します。 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

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

   Cloud Manager プログラムがネットワークインフラストラクチャをプロビジョニングするまで、45 ～ 60 分待ちます。

1. 環境が完了したことを確認します。 __仮想プライベートネットワーク__ Cloud Manager API を使用した設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 前の手順で createNetworkInfrastructure HTTP リクエストから返されました。

   __getNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_BEARER_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 応答に __ステータス__ / __準備完了__. まだ準備ができていない場合は、数分ごとにステータスを再確認します。

## 環境ごとの仮想プライベートネットワークプロキシの構成

1. を有効にして設定します。 __仮想プライベートネットワーク__ Cloud Manager API を使用した各AEMas a Cloud Service環境での設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./vpn-configure.json
   ```

   JSON パラメーターを `vpn-configure.json` を介してカールするために提供されます。 `... -d @./vpn-configure.json`.

[vpn-configure.json の例をダウンロードします。](./assets/vpn-configure.json)

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

   `nonProxyHosts` ポート 80 または 443 を専用のエグレス IP ではなく、デフォルトの共有 IP アドレス範囲を通じてルーティングする必要があるホストのセットを宣言します。 `nonProxyHosts` は、共有 IP を介したトラフィックエグレスが、Adobeによって自動的に最適化される場合に役立ちます。

   各 `portForwards` マッピングでは、アドバンスドネットワークは次の転送ルールを定義します。

   | プロキシホスト | プロキシポート |  | 外部ホスト | 外部ポート |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEMデプロイメントの場合 __のみ__ 外部サービスへの HTTP/HTTPS 接続が必要です。 `portForwards` 配列が空です。これらのルールは非 HTTP/HTTPS リクエストに対してのみ必要となるためです。


1. 各環境で、Cloud Manager API の [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 仮想プライベートネットワークプロキシの設定は、Cloud Manager API の [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 記憶する `enableEnvironmentAdvancedNetworkingConfiguration` は `PUT` 操作の場合は、この操作を呼び出すたびに、すべてのルールを指定する必要があります。

1. これで、カスタムAEMコードと設定で Virtual Private Network エグレス設定を使用できます。

## 仮想プライベートネットワーク経由での外部サービスへの接続

Virtual Private Network が有効になっている場合、AEMのコードと設定を使用して、VPN 経由で外部サービスに対する呼び出しを行うことができます。 外部呼び出しには、AEMでの処理方法が 2 種類あります。

1. 非標準ポート上の外部サービスへの HTTP/HTTPS 呼び出し
   + 標準の 80 または 443 ポート以外のポートで動作するサービスに対しておこなわれる HTTP/HTTPS 呼び出しが含まれます。
1. 外部サービスへの非 HTTP/HTTPS 呼び出し
   + HTTP 以外の呼び出し（メールサーバーとの接続、SQL データベース、HTTP/HTTPS 以外のプロトコルで実行されるサービスなど）が含まれます。

標準ポート(80/443)上のAEMからの HTTP/HTTPS リクエストは、デフォルトで許可されており、追加の設定や考慮事項は不要です。

### 非標準ポートでの HTTP/HTTPS

AEMから非標準ポート (-80/443ではなく ) への HTTP/HTTPS 接続を作成する場合は、プレースホルダーを介して提供される特別なホストとポートを介して接続を行う必要があります。

AEMは、AEM HTTP/HTTPS プロキシにマッピングされる 2 組の特別な Java™システム変数を提供します。

|変数名 |使用 | Java™ code | OSGi 設定 | Apache Web サーバー mod_proxy 設定 | | - | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | HTTP 接続のプロキシホスト | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | `${AEM_HTTP_PROXY_HOST}` | | `AEM_HTTP_PROXY_PORT` | HTTP 接続のプロキシポート | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` |  `${AEM_HTTP_PROXY_PORT}` | | `AEM_HTTPS_PROXY_HOST` | HTTPS 接続のプロキシホスト | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | `${AEM_HTTPS_PROXY_HOST}` | | `AEM_HTTPS_PROXY_PORT` | HTTPS 接続のプロキシポート | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` | `${AEM_HTTPS_PROXY_PORT}` |

HTTP/HTTPS 外部サービスへのリクエストは、AEMプロキシ hosts/ports の値を使用して Java™ HTTP クライアントのプロキシ設定を設定することでおこなう必要があります。

非標準ポートで外部サービスに対して HTTP/HTTPS 呼び出しをおこなう場合、対応するポートがない `portForwards` は、Cloud Manager API を使用して定義する必要があります `__enableEnvironmentAdvancedNetworkingConfiguration` ポート転送の「ルール」が「コード内」で定義されているので、操作を実行します。

>[!TIP]
>
> 詳しくは、 AEMas a Cloud Serviceの Virtual Private Network のドキュメントを参照してください。 [ルーティングルールの完全なセット](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#vpn-traffic-routing).

#### コードの例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports.md"><img alt="非標準ポートでの HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports.md">非標準ポートでの HTTP/HTTPS</a></strong></div>
    <p>
        AEMから非標準の HTTP/HTTPS ポート上の外部サービスに HTTP/HTTPS 接続をas a Cloud Service的にする Java™コードの例です。
    </p>
</td>
<td></td>
<td></td>
</tr>
</table>

### 非 HTTP/HTTPS 接続コードの例

HTTP/HTTPS 以外の接続を作成する場合 ( 例： AEMからの SQL、SMTP など )、接続は、AEMから提供される特別なホスト名を使用しておこなう必要があります。

|変数名 |使用 | Java™ code | OSGi 設定 | | - | - | - | - | | `AEM_PROXY_HOST` |非 HTTP/HTTPS 接続のプロキシホスト | `System.getenv("AEM_PROXY_HOST")` | `$[env:AEM_PROXY_HOST]` |


外部サービスへの接続は、 `AEM_PROXY_HOST` とマッピングされたポート (`portForwards.portOrig`) を使用し、AEMはマッピングされた外部ホスト名 (`portForwards.name`) およびポート (`portForwards.portDest`) をクリックします。

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
            Java™の SQL API を使用した外部 SQL データベースへの接続に関する Java™コードの例です。
      </p>
    </td>
   <td>
      <a  href="./examples/email-service.md"><img alt="仮想プライベートネットワーク（VPN）" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">電子メールサービス</a></strong></div>
      <p>
        外部の電子メールサービスに接続するためにAEMを使用する OSGi 設定例です。
      </p>
    </td>
</tr></table>

### VPN 経由でas a Cloud ServiceのAEMへのアクセスを制限

Virtual Private Network の設定は、AEMas a Cloud Service環境へのアクセスを VPN に制限します。

#### 設定例

<table><tr>
   <td>
      <a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html"><img alt="IP許可リストの適用" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html">IP の適許可リスト用</a></strong></div>
      <p>
            VPN トラフィックのみがAEMにアクセスできるように IP許可リストを設定します。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections"><img alt="AEM パブリッシュへのパスベースの VPN アクセス制限" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections">AEM パブリッシュへのパスベースの VPN アクセス制限</a></strong></div>
      <p>
            AEM パブリッシュ上の特定のパスに対して VPN アクセスを要求します。
      </p>
    </td>
   <td></td>
</tr></table>
