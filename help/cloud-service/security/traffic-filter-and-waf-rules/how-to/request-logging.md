---
title: 機密性の高いリクエストの監視
description: AEM as a Cloud Serviceのトラフィックフィルタールールを使用して機密リクエストをログに記録して監視する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18311
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 34%

---

# 機密性の高いリクエストの監視

AEM as a Cloud Serviceのトラフィックフィルタールールを使用して機密リクエストをログに記録して監視する方法を説明します。

ログを使用すると、エンドユーザーやサービスに影響を与えずにトラフィックパターンを監視できます。これは、ブロッキングルールを実装する前の重要な最初のステップです。

このチュートリアルでは、AEM パブリッシュサービスに対して **WKND ログインパスおよびログアウトパスのリクエストをログに記録** する方法について説明します。

## リクエストをログに記録する理由とタイミング

特定のリクエストをログに記録することは、ユーザー（潜在的に悪意のあるアクター）がAEM アプリケーションとどのようにやり取りしているかを理解するための、リスクが低く、価値の高いプラクティスです。 これは、ブロッキングルールを適用する前に特に便利です。正規のトラフィックを中断することなく、セキュリティの姿勢を確実に調整できます。

ログの一般的なシナリオは次のとおりです。

- ルールを `block` モードに昇格させる前に、ルールの影響とリーチを検証します。
- 異常なパターンやブルートフォースの試行に対するログイン/ログアウトパスおよび認証エンドポイントの監視。
- 潜在的な不正使用や DoS アクティビティに対する API エンドポイントへの高頻度アクセスのトラッキング。
- より厳密なコントロールを適用する前に、ボットの動作のベースラインを確立します。
- セキュリティインシデントが発生した場合は、フォレンジックデータを提供して攻撃の性質と影響を受けるリソースを把握します。

## 前提条件

先に進む前に、「トラフィックフィルターとWAF ルールの設定方法 [ チュートリアルの説明に従って必要な設定を完了していることを確認し ](../setup.md) ください。 また、[AEM WKND サイトプロジェクト ](https://github.com/adobe/aem-guides-wknd) のクローンを作成してAEM環境にデプロイしました。

## 例：WKND のログインリクエストとログアウトリクエストを記録する

この例では、トラフィックフィルタールールを作成して、AEM Publish サービスの WKND ログインパスおよびログアウトパスに対して行われたリクエストをログに記録します。 認証試行を監視し、潜在的なセキュリティの問題を特定するのに役立ちます。

- 次のルールを WKND プロジェクトの `/config/cdn.yaml` ファイルに追加します。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  trafficFilters:
    rules:
    # On AEM Publish service log WKND Login and Logout requests
    - name: publish-auth-requests
      when:
        allOf:
          - reqProperty: tier
            matches: publish
          - reqProperty: path
            in:
              - /system/sling/login/j_security_check
              - /system/sling/logout
      action: log   
```

- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。

- Cloud Manager設定パイプラインを使用して [ 以前に作成した ](../setup.md#deploy-rules-using-adobe-cloud-manager) 変更内容をAEM環境にデプロイします。

- プログラムの WKND サイト（例：`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html`）にログインし、サイトからサインアウトすることで、ルールをテストします。 ユーザー名とパスワードとして `asmith/asmith` を使用できます。

  ![WKND ログイン](../assets/how-to/wknd-login.png)

## 分析

Cloud Managerから AEMCS CDN ログをダウンロードし、`publish-auth-requests`AEMCS CDN ログ分析ツール [ を使用して、](../setup.md#setup-the-elastic-dashboard-tool) ルールの結果を分析しましょう。

- [Cloud Manager](https://my.cloudmanager.adobe.com/) の&#x200B;**環境**&#x200B;カードから、AEMCS **パブリッシュ**&#x200B;サービスの CDN ログをダウンロードします。

  ![Cloud Manager CDN ログのダウンロード](../assets/how-to/cloud-manager-cdn-log-downloads.png)

  >[!TIP]
  >
  > 新しいリクエストが CDN ログに表示されるまでに最大 5 分かかる場合があります。

- ダウンロードしたログファイル（例：以下のスクリーンショットの `publish_cdn_2023-10-24.log`）を Elastic ダッシュボードツールプロジェクトの `logs/dev` フォルダーにコピーします。

  ![ELK ツールログフォルダー](../assets/how-to/elk-tool-logs-folder.png)

- Elastic ダッシュボードツールページを更新します。
   - 上部の「**グローバルフィルター**」セクションで、`aem_env_name.keyword` フィルターを編集し、`dev` 環境値を選択します。

     ![ELK ツールグローバルフィルター](../assets/how-to/elk-tool-global-filter.png)

   - 時間間隔を変更するには、右上隅にあるカレンダーアイコンをクリックし、目的の時間間隔を選択します。

     ![ELK ツールの時間間隔](../assets/how-to/elk-tool-time-interval.png)

- 更新されたダッシュボードの&#x200B;**分析済みリクエスト**、**フラグ付きリクエスト**、**フラグ付きリクエストの詳細**&#x200B;パネルを確認します。一致する CDN ログエントリの場合、各エントリのクライアント IP（cli_ip）、ホスト、URL、アクション（waf_action）、ルール名（waf_match）の値が表示されます。

  ![ELK ツールダッシュボード](../assets/how-to/elk-tool-dashboard.png)

