---
title: 高度なネットワーク機能
description: AEM as a Cloud Service の高度なネットワークオプションについて説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.png
last-substantial-update: 2022-10-13T00:00:00Z
exl-id: d1c1a3cf-989a-4693-9e0f-c1b545643e41
duration: 85
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 100%

---

# 高度なネットワーク機能

AEM as a Cloud Service は、AEM as a Cloud Service プログラムとの接続を正確に管理できる高度なネットワークを提供します。

|                                                   | [実稼動プログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs.html?lang=ja) | [サンドボックスプログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-sandbox-programs.html?lang=ja) |
|---------------------------------------------------|:-----------------------:|:---------------------:|
| 高度なネットワークをサポート | ✔ | ✘ |


AEM の高度なネットワークは、外部サービスとの接続を管理する 3 つのオプションで構成されています。 Cloud Manager プログラムとその AEM as a Cloud Service 環境では、一度に 1 つのタイプの高度なネットワーク設定のみを使用できるので、最も適切なタイプが選択されていることを確認します。

|                                   | 標準ポートの HTTP／HTTPS | 非標準ポートでの HTTP/HTTPS | HTTP／HTTPS 以外の接続 | 出力専用 IP | 「No-proxy hosts」リスト | VPN 保護サービスに接続する | IP で AEM 公開トラフィックを制限する |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __高度なネットワークなし__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__柔軟なポートエグレス__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__専用エグレス IP アドレス__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__仮想プライベートネットワーク__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


適切な高度ネットワークタイプを選択する際の考慮事項の詳細については、[高度なネットワークドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html?lang=ja)を参照してください。

## 高度なネットワークチュートリアル

組織のニーズに基づく最も適切な高度なネットワークオプションを特定したら、以下の対応するチュートリアルをクリックして、手順とコードサンプルを確認します。

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="柔軟なポートエグレス" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">柔軟なポートエグレス</a></strong></div>
      <p>
          非標準ポートでの AEM as a Cloud Service 送信トラフィックを許可します。
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="専用の出力 IP アドレス" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">専用エグレス IP アドレス</a></strong></div>
      <p>
        専用 IP から AEM as a Cloud Service 送信トラフィックを発信します。
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="仮想プライベートネットワーク（VPN）" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">仮想プライベートネットワーク（VPN）</a></strong></div>
      <p>
        顧客またはベンダーのインフラストラクチャと AEM as a Cloud Service 間のトラフィックを保護します。
      </p>
    </td>   
  </tr>
</table>

## コードの例

このコレクションでは、特定の使用例で高度なネットワーク機能を利用するために必要な設定例とコードを示します。

これらのチュートリアルに従う前に、適切な[高度なネットワーク設定](#advanced-networking)が設定されているようにしてください。

<table><tr>
   <td>
      <a  href="./examples/email-service.md"><img alt="仮想プライベートネットワーク（VPN）" src="./assets/code-examples__email.png"/></a>
      <div><strong><a href="./examples/email-service.md">メールサービス</a></strong></div>
      <p>
        外部のメールサービスに接続するために AEM を使用する OSGi 設定例です。
      </p>
    </td>  
    <td>
        <a  href="./examples/http-dedicated-egress-ip-vpn.md"><img alt="HTTP／HTTPS" src="./assets/code-examples__http.png"/></a>
        <div><strong><a href="./examples/http-dedicated-egress-ip-vpn.md">HTTP／HTTPS</a></strong></div>
        <p>
            HTTP／HTTPS プロトコルを使用して、AEM as a Cloud Service から外部サービスへの HTTP／HTTPS 接続を作成する Java™ コードの例。
        </p>
    </td>
    <td>
      <a  href="./examples/sql-datasourcepool.md"><img alt="JDBC DataSourcePool を使用した SQL 接続" src="./assets//code-examples__sql-osgi.png"/></a>
      <div><strong><a href="./examples/sql-datasourcepool.md">JDBC DataSourcePool を使用した SQL 接続</a></strong></div>
      <p>
            AEM JDBC データソースプールを設定して外部 SQL データベースに接続する Java™コード例。
      </p>
    </td>   
    </tr><tr>
    <td>
      <a  href="./examples/sql-java-apis.md"><img alt="Java API を使用した SQL 接続" src="./assets/code-examples__sql-java-api.png"/></a>
      <div><strong><a href="./examples/sql-java-apis.md">Java™ API を使用した SQL 接続</a></strong></div>
      <p>
            Java™ の SQL API を使用して外部 SQL データベースに接続する Java™ コードの例。
      </p>
    </td>   
    <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=ja"><img alt="IP 許可リストの適用" src="./assets/code_examples__vpn-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/ip-allow-lists/apply-allow-list.html?lang=ja">IP 許可リストの適用</a></strong></div>
      <p>
            VPN トラフィックのみが AEM にアクセスできるように IP 許可リストを設定します。
      </p>
    </td>
   <td>
      <a  href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections?lang=ja"><img alt="AEM パブリッシュへのパスベースの VPN アクセス制限" src="./assets/code_examples__vpn-path-allow-list.png"/></a>
      <div><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html#restrict-vpn-to-ingress-connections?lang=ja">AEM パブリッシュへのパスベースの VPN アクセス制限</a></strong></div>
      <p>
            AEM パブリッシュ上の特定のパスに対して VPN アクセスを要求します。
      </p>
    </td>
</tr>
</table>
