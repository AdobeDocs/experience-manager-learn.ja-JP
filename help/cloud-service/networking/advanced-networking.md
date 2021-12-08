---
title: 高度なネットワーク
description: AEMas a Cloud Serviceの高度なネットワークオプションについて説明します。
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
source-git-commit: 6f047a76693bc05e64064fce6f25348037749f4c
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 0%

---


# 高度なネットワーク

AEM as a Cloud Serviceには、外部サービスとの接続を管理する 3 つのオプションが用意されています。 Cloud Manager プログラムとそのAEMas a Cloud Service環境では、一度に 1 つのタイプの高度なネットワーク設定のみを使用できるので、最も適切なタイプが選択されていることを確認します。

|  | 標準ポートの HTTP/HTTPS | 非標準ポートでの HTTP/HTTPS | 非 HTTP/HTTPS 接続 | 出力専用 IP | 「No-proxy hosts」リスト | VPN 保護サービスに接続 | IP で AEM 公開トラフィックを制限 |
|-----------------------------------|:----------------------------:|:--------------------------------:|:--------------------------:|:-------------------:|:-------------------------------------:|:-------------------------------------:|:----:|
| __高度なネットワークがありません__ | ✔ | ✘ | ✘ | ✘ | ✘ | ✘ | ✘ |
| [__柔軟なポート出力__](./flexible-port-egress.md) | ✔ | ✔ | ✔ | ✘ | ✘ | ✘ | ✘ |
| [__出力専用 IP アドレス__](./dedicated-egress-ip-address.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✘ | ✘ |
| [__仮想プライベートネットワーク__](./vpn.md) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |


適切なアドバンスドネットワークタイプを選択する際の考慮事項の詳細については、 [高度なネットワークドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/security/configuring-advanced-networking.html).

## 高度なネットワークチュートリアル

組織のニーズに基づく最も適切な高度なネットワークオプションを特定したら、以下の対応するチュートリアルをクリックして、手順とコードサンプルを確認します。

<table>
  <tr>
   <td>
      <a  href="./flexible-port-egress.md"><img alt="柔軟なポート出力" src="./assets/flexible-port-egress.png"/></a>
      <div><strong><a href="./flexible-port-egress.md">柔軟なポート出力</a></strong></div>
      <p>
          非標準ポートでの送信AEMas a Cloud Serviceトラフィックを許可します。
      </p>
    </td>   
   <td>
      <a  href="./dedicated-egress-ip-address.md"><img alt="ファイル専用の出力 IP アドレス" src="./assets/dedicated-egress-ip-address.png"/></a>
      <div><strong><a href="./dedicated-egress-ip-address.md">出力専用 IP アドレス</a></strong></div>
      <p>
        専用 IP から送信AEMas a Cloud Serviceトラフィックを発信します。
      </p>
    </td>   
   <td>
      <a  href="./vpn.md"><img alt="仮想プライベートネットワーク (VPN)" src="./assets/vpn.png"/></a>
      <div><strong><a href="./vpn.md">仮想プライベートネットワーク (VPN)</a></strong></div>
      <p>
        顧客またはベンダーのインフラストラクチャとAEM as a Cloud Service間のトラフィックを保護します。
      </p>
    </td>   
  </tr>
</table>
