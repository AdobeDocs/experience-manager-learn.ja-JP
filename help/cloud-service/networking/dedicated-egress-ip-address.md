---
title: 専用エグレス IP アドレス
description: 専用のエグレス IP アドレスを設定して使用する方法を説明します。このアドレスを使用すると、AEM からの送信接続を専用の IP から発信できます。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: KT-9351.jpeg
exl-id: 311cd70f-60d5-4c1d-9dc0-4dcd51cad9c7
duration: 926
source-git-commit: 970093bb54046fee49e2ac209f1588e70582ab67
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 100%

---

# 専用エグレス IP アドレス

専用のエグレス IP アドレスを設定して使用する方法を説明します。このアドレスを使用すると、AEM からの送信接続を専用の IP から発信できます。

## 専用のエグレス IP アドレスとは

専用のエグレス IP アドレスを使用すると、AEM as a Cloud Service からのリクエストで専用の IP アドレスを使用でき、外部サービスはこの IP アドレスで受信リクエストをフィルタリングできるようになります。 [フレキシブルエグレスポート](./flexible-port-egress.md)と同様、専用のエグレス IP を使用すると非標準ポートに出力できます。

Cloud Manager プログラムでは、__単一の__&#x200B;ネットワークインフラストラクチャタイプのみを持つことができます。次のコマンドを実行する前に、専用のエグレス IP アドレスが AEM as a Cloud Service に最も[適切なタイプのネットワークインフラストラクチャ](./advanced-networking.md)であることを確認してください。

>[!MORELIKETHIS]
>
> 専用のエグレス IP アドレスの詳細については、AEM as a Cloud Service の[高度なネットワーク設定ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=ja#dedicated-egress-IP-address)をお読みください。

## 前提条件

専用のエグレス IP アドレスを設定する場合は、次が必要です。

+ [Cloud Manager のビジネス所有者権限](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/permissions/)がある Cloud Manager API
+ [Cloud Manager API 認証資格情報](https://developer.adobe.com/experience-cloud/cloud-manager/guides/getting-started/create-api-integration/)にアクセスします
   + 組織 ID（別名 IMS 組織 ID）
   + クライアント ID（API キー）
   + アクセストークン（Bearer トークン）
+ Cloud Manager プログラム ID
+ Cloud Manager 環境 ID

詳しくは、次の Cloud Manager API 資格情報の設定、構成、取得方法、およびそれらを使用した Cloud Manager API 呼び出しの作成方法に関するチュートリアルを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/342235?quality=12&learn=on)

このチュートリアルでは、`curl` を使用して Cloud Manager API を設定します。指定された `curl` コマンドは、Linux／macOS 構文を想定しています。 Windows のコマンドプロンプトを使用する場合は、`\` 改行文字を `^` で置換します。

## プログラムの専用のエグレス IP アドレスを有効にする

まず、AEM as a Cloud Service で専用のエグレス IP アドレスを有効にして設定します。

1. まず、Cloud Manager API [listRegions](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して、詳細ネットワークが必要な地域を決定します。`region name` は後続の Cloud Manager API 呼び出しを行うために必要です。 通常、実稼動環境が存在する地域が使用されます。

   [Cloud Manager](https://my.cloudmanager.adobe.com) で、[環境の詳細](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html?lang=ja#viewing-environment)の下にある AEM as a Cloud Service 環境の地域を見つけます。Cloud Manager に表示される地域名は、Cloud Manager API で使用される[地域コードにマッピング](https://developer.adobe.com/experience-cloud/cloud-manager/guides/api-usage/creating-programs-and-environments/#creating-aem-cloud-service-environments?lang=ja)できます。

   __listRegions HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/regions \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' 
   ```

1. Cloud Manager API の [createNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して、Cloud Manager プログラム用の専用のエグレス IP アドレスを有効にします。Cloud Manager API の `listRegions` 操作から取得した適切な `region` コードを使用します。

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

1. Cloud Manager API の [getNetworkInfrastructure](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#operation/getNetworkInfrastructure) 操作を使用して、プログラムが&#x200B;__専用のエグレス IP アドレス__ の設定を完了したことを確認し、前の手順で createNetworkInfrastructure HTTP リクエストから返された `id` を使用します。

   __getNetworkInfrastructure HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/networkInfrastructure/{networkInfrastructureId} \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

   HTTP 応答に、__準備完了__&#x200B;の&#x200B;__ステータス__&#x200B;が含まれていることを確認します。まだ準備完了ではない場合は、数分ごとにステータスを再確認します。

## 環境ごとの専用のエグレス IP アドレスプロキシの設定

1. Cloud Manager API の [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して、各 AEM as a Cloud Service 環境で&#x200B;__専用のエグレス IP アドレス__&#x200B;設定を行います。

   __enableEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X PUT https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: Bearer <ACCESS_TOKEN>' \
       -H 'Content-Type: application/json' \
       -d @./dedicated-egress-ip-address.json
   ```

   `dedicated-egress-ip-address.json` で JSON パラメーターを定義し、`... -d @./dedicated-egress-ip-address.json` を介して curl に提供します。

   [サンプルの dedicated-egress-ip-address.json をダウンロードします](./assets/dedicated-egress-ip-address.json)。このファイルは例に過ぎません。 [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) に記載されているオプション／必須フィールドに基づいて、必要に応じてファイルを設定します。 

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

   専用のエグレス IP アドレス設定の HTTP 署名は、オプションの `nonProxyHosts` 設定もサポートするという点のみ、[柔軟なエグレスポート](./flexible-port-egress.md#enable-dedicated-egress-ip-address-per-environment)と異なります。

   `nonProxyHosts` は、ポート 80 または 443 が専用のエグレス IP ではなくデフォルトの共有 IP アドレス範囲を介してルーティングされるホストのセットを宣言します。`nonProxyHosts` は、共有 IP を介して送信されたトラフィックがアドビによって自動的に最適化される場合に役立ちます。

   各 `portForwards` マッピングでは、詳細ネットワークは次の転送ルールを定義します。

   | プロキシホスト | プロキシポート |  | 外部ホスト | 外部ポート |
   |---------------------------------|----------|----------------|------------------|----------|
   | `AEM_PROXY_HOST` | `portForwards.portOrig` | → | `portForwards.name` | `portForwards.portDest` |

1. 各環境で、Cloud Manager API の [getEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して送信ルールが有効であることを検証します。

   __getEnvironmentAdvancedNetworkingConfiguration HTTP リクエスト__

   ```shell
   $ curl -X GET https://cloudmanager.adobe.io/api/program/{programId}/environment/{environmentId}/advancedNetworking \
       -H 'x-gw-ims-org-id: <ORGANIZATION_ID>' \
       -H 'x-api-key: <CLIENT_ID>' \
       -H 'Authorization: <YOUR_TOKEN>' \
       -H 'Content-Type: application/json'
   ```

1. 専用のエグレス IP アドレス設定は、Cloud Manager API の [enableEnvironmentAdvancedNetworkingConfiguration](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/) 操作を使用して更新できます。`enableEnvironmentAdvancedNetworkingConfiguration` は `PUT` 操作であるため、この操作を呼び出すたびにすべてのルールを提供する必要があることに注意してください。

1. ホスト：`p{programId}.external.adobeaemcloud.com` で DNS リゾルバー（[DNSChecker.org](https://dnschecker.org/) など）を使用するか、コマンドラインから `dig` を実行して、__専用のエグレス IP アドレス__&#x200B;を取得します。

   ```shell
   $ dig +short p{programId}.external.adobeaemcloud.com
   ```

   ホスト名を `pinged` にすることはできません。これはエグレスであり、イングレス&#x200B;_ではない_&#x200B;からです。

   __専用のエグレス IP アドレス__&#x200B;は、プログラム内のすべての AEM as a Cloud Service 環境で共有されることに注意してください。

1. これで、カスタム AEM コードおよび設定で、専用のエグレス IP アドレスを使用できるようになりました。多くの場合、専用のエグレス IP アドレスを使用する場合、外部サービスの AEM as a Cloud Service 接続先は、この専用 IP アドレスからのトラフィックのみを許可するように設定されます。

## 専用のエグレス IP アドレスを使用した外部サービスへの接続

専用のエグレス IP アドレスを有効にすると、AEM のコードと設定で、専用のエグレス IP を使用して外部サービスを呼び出すことができます。 外部呼び出しには、AEM での処理方法が 2 種類あります。

1. 外部サービスへの HTTP／HTTPS 呼び出し
   + 標準の 80 または 443 ポート以外のポートで動作するサービスに対して行われる HTTP／HTTPS 呼び出しが含まれます。
1. 外部サービスへの HTTP／HTTPS 以外の呼び出し
   + HTTP 以外の呼び出し（メールサーバーとの接続、SQL データベース、HTTP／HTTPS 以外のプロトコルで実行されるサービスなど）が含まれます。

標準ポート（80／443）上の AEM からの HTTP／HTTPS リクエストはデフォルトで許可されていますが、以下に説明するように適切に設定されていない場合は、専用のエグレス IP アドレスを使用しません。

>[!TIP]
>
> [ルーティングルールの完全なセット](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/configuring-advanced-networking.html?lang=ja#dedcated-egress-ip-traffic-routing=)については、AEM as a Cloud Service の専用エグレス IP アドレスのドキュメントを参照してください。


### HTTP／HTTPS

AEM から HTTP／HTTPS 接続を作成するときに、専用のエグレス IP アドレスを使用すると、HTTP／HTTPS 接続は専用のエグレス IP アドレスを使用して AEM から自動的にプロキシされます。HTTP／HTTPS 接続をサポートするために、追加のコードや設定は必要ありません。

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

### 外部サービスへの HTTP／HTTPS 以外の接続

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
        外部のメールサービスに接続するために AEM を使用する OSGi 設定例です。
      </p>
    </td>   
</tr></table>
