---
title: 柔軟なポート出力
description: 柔軟なポート出力を設定して使用し、AEMas a Cloud Serviceから外部サービスへの外部接続をサポートする方法について説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9350
thumbnail: KT-9350.jpeg
exl-id: 5c1ff98f-d1f6-42ac-a5d5-676a54ef683c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 5%

---

# 柔軟なポート出力

柔軟なポート出力を設定して使用し、AEMas a Cloud Serviceから外部サービスへの外部接続をサポートする方法について説明します。

## フレキシブルポートエグレスとは

柔軟なポートエグレスにより、AEMas a Cloud Serviceのカスタムの特定のポート転送ルールを接続し、AEMから外部サービスへの接続を可能にします。

Cloud Manager プログラムでは、 __シングル__ ネットワークインフラストラクチャのタイプ。 出力専用 IP アドレスが最も多いことを確認する [適切な種類のネットワークインフラストラクチャ](./advanced-networking.md)  次のコマンドを実行する前にAEMas a Cloud Serviceのに対して実行します。

>[!MORELIKETHIS]
>
> AEMを読む [高度なネットワーク設定ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress) フレキシブルポートエグレスの詳細については、を参照してください。

## 前提条件

フレキシブルポートエグレスを設定する場合は、次の操作が必要です。

+ Cloud Manager API を有効にしたAdobe Developer Console プロジェクト [Cloud Manager のビジネスオーナー権限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)
+ アクセス先 [Cloud Manager API の認証資格情報](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/authentication/)
   + 組織 ID（別名 IMS Org ID）
   + クライアント ID（別名 API キー）
   + アクセストークン（Bearer トークン）
+ Cloud Manager プログラム ID
+ Cloud Manager 環境 ID

詳しくは、次の Cloud Manager API 資格情報の設定、設定、取得方法、およびそれらを使用した Cloud Manager API 呼び出しの作成方法に関するチュートリアルを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

このチュートリアルでは、 `curl` Cloud Manager API を設定する場合。 指定された `curl` コマンドは、Linux/macOS構文を想定しています。 Windows のコマンドプロンプトを使用する場合は、 `\` 改行文字 `^`.

## プログラムごとの柔軟なポート出力の有効化

まず、AEMのフレキシブルポートエグレスを有効にします。

1. まず、Cloud Manager API を使用して、で Advanced Networking が設定されている地域を特定します。 [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 この `region name` 後続の Cloud Manager API 呼び出しをおこなうには、が必要です。 通常、実稼動環境が存在する地域が使用されます。

   でAEMas a Cloud Service環境の地域を見つけます。 [Cloud Manager](https://my.cloudmanager.adobe.com) の下に [環境の詳細](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=en#viewing-environment). Cloud Manager に表示される地域名は、次のとおりです。 [地域コードにマッピング済み](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments) Cloud Manager API で使用されます。

   __listRegions HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Cloud Manager API を使用して、Cloud Manager プログラムの柔軟なポート出力を有効にします [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 適切な `region` Cloud Manager API から取得したコード `listRegions` 操作。

   __createNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X POST https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructures \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d '{ "kind": "flexiblePortEgress", "region": "va7" }'
   ```

   Cloud Manager プログラムがネットワークインフラストラクチャをプロビジョニングするまで 15 分待ちます。

1. 環境が完了したことを確認します。 __フレキシブルポートエグレス__ Cloud Manager API を使用した設定 [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作，使用 `id` 前の手順で createNetworkInfrastructure HTTP リクエストから返されました。

   __getNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 応答に __ステータス__ / __準備完了__. まだ準備ができていない場合は、数分ごとにステータスを再確認します。

## 環境ごとに柔軟なポート出力プロキシを設定する

1. を有効にして設定します。 __フレキシブルポートエグレス__ Cloud Manager API を使用した各AEMas a Cloud Service環境での設定 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./flexible-port-egress.json
   ```

   JSON パラメーターを `flexible-port-egress.json` を介してカールするために提供されます。 `... -d @./flexible-port-egress.json`.

   [サンプルの flexible-port-egress.json をダウンロードします。](./assets/flexible-port-egress.json). このファイルは例に過ぎません。 次のドキュメントに記載されているオプション/必須フィールドに基づいて、必要に応じてファイルを設定します。 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/).

   ```json
   {
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

   各 `portForwards` マッピングでは、アドバンスドネットワークは次の転送ルールを定義します。

   | プロキシホスト | プロキシポート |  | 外部ホスト | 外部ポート |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

   AEMデプロイメントの場合 __のみ__ 外部サービスへの HTTP/HTTPS 接続 ( ポート80/443) が必要です。 `portForwards` 配列が空です。これらのルールは非 HTTP/HTTPS リクエストに対してのみ必要となるためです。

1. 各環境で、Cloud Manager API を使用してエグレスルールが有効であることを検証します [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'x-api-key: <CLIENT_ID>' \ 
       -H 'Content-Type: application/json'
   ```

1. 柔軟なポートエグレス設定は、Cloud Manager API を使用して更新できます [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作。 記憶する `enableEnvironmentAdvancedNetworkingConfiguration` は `PUT` 操作の場合は、この操作を呼び出すたびに、すべてのルールを指定する必要があります。

1. これで、カスタムAEMコードと設定で柔軟なポートエグレス設定を使用できます。


## フレキシブルポートエグレスを介した外部サービスへの接続

柔軟なポートエグレスプロキシを有効にすると、AEMのコードと設定で、これらを使用して外部サービスを呼び出すことができます。 外部呼び出しには、AEMでの処理方法が 2 種類あります。

1. 非標準ポート上の外部サービスへの HTTP/HTTPS 呼び出し
   + 標準の 80 または 443 ポート以外のポートで動作するサービスに対しておこなわれる HTTP/HTTPS 呼び出しが含まれます。
1. 外部サービスへの非 HTTP/HTTPS 呼び出し
   + HTTP 以外の呼び出し（メールサーバーとの接続、SQL データベース、HTTP/HTTPS 以外のプロトコルで実行されるサービスなど）が含まれます。

標準ポート(80/443)上のAEMからの HTTP/HTTPS リクエストは、デフォルトで許可されており、追加の設定や考慮事項は不要です。


### 非標準ポートでの HTTP/HTTPS

AEMから非標準ポート (-80/443ではなく ) への HTTP/HTTPS 接続を作成する場合、接続は特別なホストとポートを介して行う必要があります（プレースホルダーを介して提供）。

AEMは、AEM HTTP/HTTPS プロキシにマッピングされる 2 組の特別な Java™システム変数を提供します。

|変数名 |使用 | Java™ code | OSGi 設定 | | - | - | - | - | | `AEM_PROXY_HOST` |両方の HTTP/HTTPS 接続のプロキシホスト | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` | | `AEM_HTTP_PROXY_PORT` | HTTPS 接続のプロキシポート（フォールバックを次に設定） `3128`) | `System.getenv().getOrDefault("AEM_HTTP_PROXY_PORT", 3128)` | `$[env:AEM_HTTP_PROXY_PORT;default=3128]` | | `AEM_HTTPS_PROXY_PORT` | HTTPS 接続のプロキシポート（フォールバックを次に設定） `3128`) | `System.getenv().getOrDefault("AEM_HTTPS_PROXY_PORT", 3128)` | `$[env:AEM_HTTPS_PROXY_PORT;default=3128]` |

非標準ポートで外部サービスに対して HTTP/HTTPS 呼び出しをおこなう場合、対応するポートがない `portForwards` は、Cloud Manager API を使用して定義する必要があります `enableEnvironmentAdvancedNetworkingConfiguration` ポート転送の「ルール」が「コード内」で定義されているので、操作を実行します。

>[!TIP]
>
> 詳しくは、 AEMas a Cloud Serviceの柔軟なポート出力に関するドキュメントを参照してください。 [ルーティングルールの完全なセット](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html#flexible-port-egress-traffic-routing).

#### コードの例

<table>
<tr>
<td>
    <a  href="./examples/http-on-non-standard-ports-flexible-port-egress.md"><img alt="非標準ポートでの HTTP/HTTPS" src="./assets/code-examples__http.png"/></a>
    <div><strong><a href="./examples/http-on-non-standard-ports-flexible-port-egress.md">非標準ポートでの HTTP/HTTPS</a></strong></div>
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

|変数名 |使用 | Java™ code | OSGi 設定 | | - | - | - | - | | `AEM_PROXY_HOST` |非 HTTP/HTTPS 接続のプロキシホスト | `System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel")` | `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` |


外部サービスへの接続は、 `AEM_PROXY_HOST` とマッピングされたポート (`portForwards.portOrig`) を使用し、AEMはマッピングされた外部ホスト名 (`portForwards.name`) およびポート (`portForwards.portDest`) をクリックします。

| プロキシホスト | プロキシポート |  | 外部ホスト | 外部ポート |
|---------------------------------|----------|----------------|------------------|----------|
| `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

#### コードの例

<table><tr>
   <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="JDBC DataSourcePool を使用した SQL 接続" src="./assets/code-examples__sql-osgi.png"/></a>
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
