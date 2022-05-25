---
title: 出力専用 IP アドレス
description: 専用の出力 IP アドレスを設定して使用する方法を説明します。このアドレスを使用すると、AEMからの送信接続を専用の IP から発信できます。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
source-git-commit: 4f8222d3185ad4e87eda662c33c9ad05ce3b0427
workflow-type: tm+mt
source-wordcount: '1229'
ht-degree: 1%

---

# 出力専用 IP アドレス

専用の出力 IP アドレスを設定して使用する方法を説明します。このアドレスを使用すると、AEMからの送信接続を専用の IP から発信できます。

## 出力専用 IP アドレスは何ですか。

専用の出力 IP アドレスを使用すると、AEMas a Cloud Serviceからの要求で専用の IP アドレスを使用でき、外部サービスはこの IP アドレスで受信要求をフィルタリングできます。 次に類似 [フレキシブルエグレスポート](./flexible-port-egress.md)に設定すると、専用の出力 IP を使用して非標準ポートに出力できます。

Cloud Manager プログラムでは、 __シングル__ ネットワークインフラストラクチャのタイプ。 出力専用 IP アドレスが最も多いことを確認する [適切な種類のネットワークインフラストラクチャ](./advanced-networking.md)  次のコマンドを実行する前にAEMas a Cloud Serviceのに対して実行します。

>[!MORELIKETHIS]
>
> AEMを読む [高度なネットワーク設定ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedicated-egress-IP-address) を参照してください。

## 前提条件

専用の出力 IP アドレスを設定する場合は、次が必要です。

+ を使用した Cloud Manager API [Cloud Manager のビジネスオーナー権限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ アクセス先 [Cloud Manager API 認証資格情報](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織 ID（別名 IMS Org ID）
   + クライアント ID（別名 API キー）
   + アクセストークン（Bearer トークン）
+ Cloud Manager プログラム ID
+ Cloud Manager 環境 ID

詳しくは、次の Cloud Manager API 資格情報の設定、設定、取得方法、およびそれらを使用した Cloud Manager API 呼び出しの作成方法に関するチュートリアルを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/342235/?quality=12&learn=on)

このチュートリアルでは、 `curl` Cloud Manager API を設定する場合。 指定された `curl` コマンドは、Linux/macOS構文を想定しています。 Windows のコマンドプロンプトを使用する場合は、 `\` 改行文字 `^`.

## プログラムの出力専用 IP アドレスを有効にする

まず、AEM as a Cloud Serviceで専用の出力 IP アドレスを有効にして設定します。

1. まず、Cloud Manager API を使用して、アドバンスドネットワークが設定される地域を決定します [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 この `region name` 後続の Cloud Manager API 呼び出しをおこなうには、が必要です。 通常、実稼動環境が存在する地域が使用されます。

   __listRegions HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Cloud Manager API を使用して、Cloud Manager プログラム用の専用エグレス IP アドレスを有効にします [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 適切な `region` Cloud Manager API から取得したコード `listRegions` 操作。

   __createNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "dedicatedEgressIp", "region": "va7" }'
   ```

   Cloud Manager プログラムがネットワークインフラストラクチャをプロビジョニングするまで 15 分待ちます。

1. 環境が完了したことを確認します。 __出力専用 IP アドレス__ Cloud Manager API を使用した設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 前の手順で createNetworkInfrastructure HTTP リクエストから返されました。

   __getNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 応答に __ステータス__ / __準備完了__. まだ準備ができていない場合は、数分ごとにステータスを再確認します。

## 環境ごとの専用の出力 IP アドレスプロキシの設定

1. を有効にして設定します。 __出力専用 IP アドレス__ Cloud Manager API を使用した各AEMas a Cloud Service環境での設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   JSON パラメーターを `dedicated-egress-ip-address.json` を介してカールするために提供されます。 `... -d @./dedicated-egress-ip-address.json`.

   [サンプルの dedicated-egress-ip-address.json をダウンロードします。](./assets/dedicated-egress-ip-address.json). このファイルは例に過ぎません。 次のドキュメントに記載されているオプション/必須フィールドに基づいて、必要に応じてファイルを設定します。 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
       "nonProxyHosts": [
           "example.net",
           "*.example.org",
       ],
       "portForwards": [
           {
               "name": "mysql.example.com",
               "portDest": 3306,
               "portOrig": 30001
           },
           {
               "name": "smtp.sendgrid.net",
               "portDest": 465,
               "portOrig": 30002
           }
       ]
   }
   ```

   専用の出力 IP アドレス設定の HTTP 署名のみが、 [フレキシブルエグレスポート](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment) オプションの `nonProxyHosts` 設定。

   `nonProxyHosts` ポート 80 または 443 を専用のエグレス IP ではなく、デフォルトの共有 IP アドレス範囲を通じてルーティングする必要があるホストのセットを宣言します。 `nonProxyHosts` は、共有 IP を介したトラフィックエグレスが、Adobeによって自動的に最適化される場合に役立ちます。

   各 `portForwards` マッピングでは、アドバンスドネットワークは次の転送ルールを定義します。

   | プロキシホスト | プロキシポート |  | 外部ホスト | 外部ポート |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 各環境で、Cloud Manager API を使用してエグレスルールが有効であることを検証します [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 専用の出力 IP アドレス設定は、Cloud Manager API を使用して更新できます [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 記憶する `enableEnvironmentAdvancedNetworkingConfiguration` は `PUT` 操作の場合は、この操作を呼び出すたびに、すべてのルールを指定する必要があります。

1. の取得 __出力専用 IP アドレス__ DNS リゾルバー ( [DNSChecker.org](https://dnschecker.org/)) をホスト上で以下の手順に従ってください。 `p{programId}.external.adobeaemcloud.com`または実行 `dig` コマンドラインから。

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   ホスト名を `pinged`出口で _not_ そして入り口

1. これで、カスタムAEMコードおよび設定で、専用の出力 IP アドレスを使用できます。 多くの場合、専用の出力 IP アドレスを使用する場合、外部サービスのAEMas a Cloud Service接続先は、この専用 IP アドレスからのトラフィックのみを許可するように設定されます。

## 専用ポートエグレスを介した外部サービスへの接続

専用の出力 IP アドレスを有効にすると、AEMのコードと設定で、専用の出力 IP を使用して外部サービスを呼び出すことができます。 外部呼び出しには、AEMでの処理方法が 2 種類あります。

1. 非標準ポート上の外部サービスへの HTTP/HTTPS 呼び出し
   + 標準の 80 または 443 ポート以外のポートで動作するサービスに対しておこなわれる HTTP/HTTPS 呼び出しが含まれます。
1. 外部サービスへの非 HTTP/HTTPS 呼び出し
   + HTTP 以外の呼び出し（メールサーバーとの接続、SQL データベース、HTTP/HTTPS 以外のプロトコルで実行されるサービスなど）が含まれます。

標準ポート(80/443)上のAEMからの HTTP/HTTPS リクエストは、デフォルトで許可されており、追加の設定や考慮事項は不要です。

>[!TIP]
>
> 詳しくは、 AEMas a Cloud Serviceの出力 IP アドレスに関する専用ドキュメントを参照してください。 [ルーティングルールの完全なセット](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#dedcated-egress-ip-traffic-routing=).


### 非標準ポートでの HTTP/HTTPS

AEMから非標準ポート (-80/443ではなく ) への HTTP/HTTPS 接続を作成する場合、特別なホストとポートを介して接続する必要があります（プレースホルダーを介して提供）。

AEMは、AEM HTTP/HTTPS プロキシにマッピングされる 2 組の特別な Java™システム変数を提供します。

|変数名 |使用 | Java™ code | OSGi 設定 | | - | - | - | - | | `AEM_HTTP_PROXY_HOST` | HTTP 接続のプロキシホスト | `System.getenv("AEM_HTTP_PROXY_HOST")` | `$[env:AEM_HTTP_PROXY_HOST]` | | `AEM_HTTP_PROXY_PORT` | HTTP 接続のプロキシポート | `System.getenv("AEM_HTTP_PROXY_PORT")` | `$[env:AEM_HTTP_PROXY_PORT]` | | `AEM_HTTPS_PROXY_HOST` | HTTPS 接続のプロキシホスト | `System.getenv("AEM_HTTPS_PROXY_HOST")` | `$[env:AEM_HTTPS_PROXY_HOST]` | | `AEM_HTTPS_PROXY_PORT` | HTTPS 接続のプロキシポート | `System.getenv("AEM_HTTPS_PROXY_PORT")` | `$[env:AEM_HTTPS_PROXY_PORT]` |

HTTP/HTTPS 外部サービスへのリクエストは、AEMプロキシの hosts/ports 値を使用して Java™ HTTP クライアントのプロキシ設定を設定することでおこなう必要があります。

非標準ポートで外部サービスに対して HTTP/HTTPS 呼び出しをおこなう場合、対応するポートがない `portForwards` は、Cloud Manager API を使用して定義する必要があります `enableEnvironmentAdvancedNetworkingConfiguration` ポート転送の「ルール」が「コード内」で定義されているので、操作を実行します。

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

### 外部サービスへの HTTP/HTTPS 以外の接続

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
