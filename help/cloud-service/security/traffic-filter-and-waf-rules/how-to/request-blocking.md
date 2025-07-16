---
title: アクセスの制限
description: AEM as a Cloud Serviceでトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18312
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '390'
ht-degree: 22%

---

# アクセスの制限

AEM as a Cloud Serviceでトラフィックフィルタールールを使用して特定のリクエストをブロックし、アクセスを制限する方法を説明します。

このチュートリアルでは、AEM Publish サービスで **パブリック IP から内部パスへのリクエストをブロック** する方法について説明します。

## リクエストをブロックする理由とタイミング

トラフィックのブロックは、特定の条件下で機密性の高いリソースや URL へのアクセスを防ぐことで、組織のセキュリティポリシーを強化するのに役立ちます。 ロギングと比較すると、ブロッキングはより厳密なアクションで、特定のソースからのトラフィックが不正または不要であると確信できる場合に使用する必要があります。

ブロックが適切な一般的なシナリオは次のとおりです。

- `internal` ページまたは `confidential` ページへのアクセスを内部 IP 範囲のみに制限する（例えば、企業 VPN の背後など）。
- ボットトラフィック、自動スキャナー、または IP またはジオロケーションで識別される脅威アクターをブロックします。
- ステージングされた移行中に非推奨（廃止予定）または保護されていないエンドポイントへのアクセスを防止する。
- パブリッシュ層でのオーサリングツールまたは管理ルートへのアクセスを制限します。

## 前提条件

続行する前に、[ トラフィックフィルターとWAF ルールの設定方法 ](../setup.md) チュートリアルの説明に従って、必要な設定を完了していることを確認してください。 また、[AEM WKND サイトプロジェクト ](https://github.com/adobe/aem-guides-wknd) のクローンを作成して、AEM環境にデプロイしました。

## 例：パブリック IP からの内部パスをブロックする

この例では、パブリック IP アドレスから `https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` などの内部 WKND ページへの外部アクセスをブロックするルールを設定します。 このページにアクセスできるのは、信頼できる IP 範囲内のユーザー（企業の VPN など）のみです。

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

- Cloud Manager設定パイプラインを使用して [ 以前に作成した ](../setup.md#deploy-rules-using-adobe-cloud-manager) 変更内容をAEM環境にデプロイします。

- WKND サイトの内部ページ（`https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html` など）にアクセスするか、または以下の CURL コマンドを使用して、ルールをテストします。

  ```bash
  $ curl -I https://publish-pXXXX-eYYYY.adobeaemcloud.com/content/wknd/internal/demo-page.html
  ```

- ルールで使用した IP アドレスと、別の IP アドレス（例えば、携帯電話を使用する場合）の両方から上記の手順を繰り返します。

## 分析中

`block-internal-paths` ルールの結果を分析するには、[ 設定チュートリアル ](../setup.md#cdn-logs-ingestion) の説明と同じ手順に従います

**ブロックされたリクエスト** と対応する値が、クライアントの IP （cli_ip）、ホスト、URL、アクション（waf_action）、ルール名（waf_match）の各列に表示されます。

![ELK ツールダッシュボードのブロックされたリクエスト](../assets/how-to/elk-tool-dashboard-blocked.png)
