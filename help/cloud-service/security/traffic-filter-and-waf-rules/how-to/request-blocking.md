---
title: アクセスの制限
description: AEM as a Cloud Service のトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
exl-id: 53cb8996-4944-4137-a979-6cf86b088d42
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 100%

---

# アクセスの制限

AEM as a Cloud Service のトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法について説明します。

このチュートリアルでは、AEM パブリッシュサービスで&#x200B;**パブリック IP からの内部パスへのリクエストをブロック**&#x200B;する方法について説明します。

## リクエストをブロックする理由とタイミング

トラフィックをブロックすると、特定の条件下で機密性の高いリソースや URL へのアクセスを防ぐことで、組織のセキュリティポリシーの適用に役立ちます。ログと比較すると、ブロックはより厳格なアクションで、特定のソースからのトラフィックが不正または不要であると確信できる場合に使用する必要があります。

ブロックが適切な一般的なシナリオは次のとおりです。

- `internal` ページや `confidential` ページへのアクセスを内部 IP 範囲にのみ制限します（例：企業 VPN の背後など）。
- IP または位置情報によって特定されるボットトラフィック、自動スキャナーまたは脅威アクターをブロックします。
- ステージングされた移行中に、非推奨（廃止予定）または安全でないエンドポイントへのアクセスを防ぎます。
- パブリッシュ層でのオーサリングツールまたは管理ルートへのアクセスを制限します。

## 前提条件

続行する前に、[トラフィックフィルタールールと WAF ルールの設定方法](../setup.md)チュートリアルの説明に従って必要な設定が完了していることを確認します。また、[AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd)のクローンを作成し、AEM 環境にデプロイしておきます。

## 例：パブリック IP からの内部パスのブロック

この例では、パブリック IP アドレスから `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` などの内部 WKND ページへの外部アクセスをブロックするルールを設定します。このページにアクセスできるのは、信頼できる IP 範囲（企業 VPN など）内のユーザーのみです。

独自の内部ページ（`demo-page.html` など）を作成することも、[添付されているパッケージ](../assets/how-to/demo-internal-pages-package.zip)を使用することもできます。

- 次のルールを WKND プロジェクトの `/config/cdn.yaml` ファイルに追加します。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # Block requests to (demo) internal only page/s from public IP address but allow from internal IP address.
    # Make sure to replace the IP address with your own IP address.
    - name: block-internal-paths
      when:
        allOf:
          - reqProperty: path
            matches: /content/wknd/internal
          - reqProperty: clientIp
            notIn: [192.150.10.0/24]
      action: block    
```

- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。

- [以前に作成した](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 設定パイプラインを使用して、変更を AEM 環境にデプロイします。

- WKND サイトの内部ページ（`https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` など）にアクセスするか、または以下の CURL コマンドを使用して、ルールをテストします。

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- ルールで使用した IP アドレスと、別の IP アドレス（例えば、携帯電話を使用する場合）の両方から上記の手順を繰り返します。

## 分析中

`block-internal-paths` ルールの結果を分析するには、[設定チュートリアル](../setup.md#cdn-logs-ingestion)で説明したのと同じ手順に従います。

**ブロックされたリクエスト**&#x200B;と、クライアント IP（cli_ip）、ホスト、URL、アクション（waf_action）、ルール名（waf_match）の各列に対応する値が表示されます。

![ELK ツールダッシュボードのブロックされたリクエスト](../assets/how-to/elk-tool-dashboard-blocked.png)
