---
title: Adobe CDN - キャッシュの範囲を超える高度な機能
description: CDN でのトラフィックの設定、トークンと資格情報の設定、CDN エラーページなど、キャッシュの範囲を超える Adobe CDN の高度な機能について説明します。
version: Experience Manager as a Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 7b29187ef84bebebd4586374abb09ced947dff28
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 95%

---

# Adobe CDN - キャッシュの範囲を超える高度な機能

CDN でのトラフィックの設定、トークンと資格情報の設定、CDN エラーページなど、キャッシュの範囲を超える Adobe コンテンツ配信ネットワーク（CDN）の高度な機能について説明します。

Adobe CDN では、コンテンツのキャッシュを超える、web サイトのパフォーマンスを最適化するのに役立つ高度な機能をいくつか提供します。これらの機能には、以下が含まれます。

- CDN でのトラフィックの設定
- CDN 資格情報および認証の設定
- CDN エラーページ

これらの機能は&#x200B;**セルフサービス**&#x200B;機能です。AEM プロジェクトの `cdn.yaml` ファイルで設定され、Cloud Manager 設定パイプラインを使用してデプロイされます。

>[!VIDEO](https://video.tv.adobe.com/v/3440270?captions=jpn&quality=12&learn=on)

## CDN でのトラフィックの設定

_CDN でのトラフィックの設定_&#x200B;に関連する次の主な機能を理解しましょう。

- **DoS 攻撃の防止：** Adobe CDN はネットワーク層で DoS 攻撃を吸収し、接触チャネルサーバーに到達するのを防ぎます。
- **レート制限：**&#x200B;接触チャネルサーバーが過剰なリクエストで過負荷になるのを防ぐために、CDN でレート制限を設定できます。
- **Web アプリケーションファイアウォール（WAF）：** WAF は、SQL インジェクション、クロスサイトスクリプティングなどの一般的な web アプリケーションの脆弱性から web サイトを保護します。この機能を使用するには、Extended Security （旧称WAF-DDoS Protection）または Extended Security for Healthcare （旧称 Enhanced Security）ライセンスが必要です。
- **リクエスト変換：**&#x200B;ヘッダーの設定や設定解除、クエリパラメーター、Cookie の変更など、受信リクエストを変更します。
- **応答変換：**&#x200B;ヘッダーの設定や設定解除など、送信応答を変更します。
- **接触チャネルの選択：**&#x200B;リクエスト URL に基づいて、トラフィックを異なる接触チャネルサーバー（アドビおよびアドビ以外）にルーティングします。
- **URL リダイレクト：**&#x200B;リクエスト（HTTP 301／302）を別の絶対 URL または相対 URL にリダイレクトします。

## CDN 資格情報および認証の設定

_CDN 資格情報と認証の設定_&#x200B;に関連する次の主な機能を理解しましょう。

- **API トークンをパージ**：キャッシュから単一、グループ、またはすべてのリソースをパージするための独自のパージキーを作成できます。
- **基本認証**：Web サイトまたはその一部へのアクセスを制限する場合の軽量の認証メカニズム。主に、公開前の様々なレビュープロセスの一部として必要です。
- **HTTP ヘッダー検証**：顧客管理 CDN でトラフィックを Adobe CDN にルーティングする際に使用されます。Adobe CDN では、`X-AEM-Edge-Key` ヘッダー値に基づいて受信リクエストを検証します。`X-AEM-Edge-Key` ヘッダーに独自の値を作成できます。

## CDN エラーページ

_CDN エラーページ_&#x200B;に関連する次の主な機能について理解しましょう。

- **ブランドのエラーページ**：Adobe CDN で接触チャネルサーバーに到達できないという&#x200B;_可能性が低いシナリオ_&#x200B;で、ブランドのエラーページをユーザーに表示します。

## 実装方法

これらの高度な機能の実装には、次の 2 つの手順が含まれます。

1. **CDN 設定ファイルを更新**：必要な設定で AEM プロジェクトの `cdn.yaml` ファイルを更新します。設定は、ルールとして追加され、ルール構文に従います。ルールの 3 つの主なコンポーネントは、`name`、`when`、`action` です。

2. **CDN 設定ファイルをデプロイ**：Cloud Manager 設定パイプラインを使用して、更新された `cdn.yaml` ファイルをデプロイします。詳しくは、[Cloud Manager を使用したルールのデプロイ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)を参照してください。

### 例

以下の例では、サンプル WKND サイトは `/top3` URL を `/us/en/top3.html` にリダイレクトするように設定されています。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## 関連チュートリアル

[トラフィックフィルタールールによる web サイトの保護](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[HTTP ヘッダー検証 CDN ルールの設定とデプロイ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[CDN キャッシュをパージする方法](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[CDN エラーページの設定](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[CDN でのトラフィックの設定](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[CDN 資格情報および認証の設定](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

