---
title: Adobe CDN - キャッシングを超える高度な機能
description: CDN でのトラフィックの設定、トークンと資格情報の設定、CDN エラーページなど、Adobe CDN のキャッシュ以外の高度な機能について説明します。
version: Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, Architect, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 8795024a7b5e6d10cb2ff2f770dd3d080af85e68
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# Adobe CDN - キャッシングを超える高度な機能

CDN でのトラフィックの設定、トークンと資格情報の設定、CDN エラーページなど、キャッシュ以外のAdobeコンテンツ配信ネットワーク（CDN）の高度な機能について説明します。

Adobe CDN には、コンテンツのキャッシュ以外に、web サイトのパフォーマンスを最適化するのに役立つ高度な機能がいくつか用意されています。 次のような機能があります。

- CDN でのトラフィックの設定
- CDN 資格情報と認証の設定
- CDN エラーページ

これらの機能は **セルフサービス** 機能です。 AEM プロジェクトの `cdn.yaml` ファイルで設定され、Cloud Manager設定パイプラインを使用してデプロイされます。

>[!VIDEO](https://video.tv.adobe.com/v/3433104?quality=12&learn=on)

## CDN でのトラフィックの設定

_CDN でのトラフィックの設定_ に関連する主な機能を説明します。

- **DoS 攻撃防止：** Adobe CDN は、ネットワーク層での DoS 攻撃を吸収し、オリジンサーバーに到達するのを防ぎます。
- **レート制限：** オリジンサーバーが過剰なリクエストによって圧倒されるのを防ぐために、CDN でレート制限を設定できます。
- **Web アプリケーションファイアウォール（WAF）:** WAFは、SQL インジェクション、クロスサイトスクリプティングなどの、一般的な web アプリケーションの脆弱性から web サイトを保護します。 この機能を使用するには、Enhanced Security ライセンスまたはWAF-DDoS Protection ライセンスが必要です。
- **リクエスト変換：** ヘッダーの設定や設定解除、クエリパラメーターの変更、cookie などの受信リクエストを変更します。
- **応答変換：** ヘッダーの設定や設定解除など、送信する応答を変更します。
- **オリジンの選択：** リクエスト URL に基づいて、異なるオリジンサーバー（Adobeと非Adobe）にトラフィックをルーティングします。
- **URL リダイレクト：** リダイレクトリクエスト （HTTP 301/302）を別の絶対 URL または相対 URL にリダイレクトします。

## CDN 資格情報と認証の設定

_CDN 資格情報と認証の設定_ に関する主な機能を説明します。

- **パージ API トークン**：単一またはグループまたはすべてのリソースをキャッシュからパージするための独自のパージキーを作成できます。
- **基本認証**:Web サイトまたはその一部へのアクセスを制限する場合に使用する、軽量の認証メカニズム。 運用開始前の様々なレビュープロセスの一環として必要となることが多い。
- **HTTP Adobe検証**：お客様が管理する CDN がヘッダー CDN にトラフィックをルーティングする場合に使用します。 Adobe CDN は、`X-AEM-Edge-Key` ヘッダー値に基づいて受信リクエストを検証します。 `X-AEM-Edge-Key` ヘッダーに独自の値を作成できます。

## CDN エラーページ

_CDN エラーページ_ に関連する主な機能を説明します。

- **ブランドエラーページ**:AdobeCDN がオリジンサーバーに到達できない場合に、_発生する可能性は低い_ シナリオでユーザーにブランドエラーページを表示します。

## 実装方法

これらの高度な機能を実装するには、次の 2 つの手順が必要です。

1. **CDN 設定ファイルを更新**:AEM プロジェクトの `cdn.yaml` ファイルを必要な設定で更新します。 設定はルールとして追加され、ルール構文に従います。 ルールは、3 つの主要なコンポーネント：`name`、`when` および `action`。

2. **CDN 設定ファイルをデプロイ**:Cloud Manager設定パイプラインを使用して、更新された `cdn.yaml` ファイルをデプロイします。 詳しくは、[Cloud Managerを使用したルールのデプロイ ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager) を参照してください。

### 例

以下の例では、サンプルの WKND サイトは、`/top3` URL を `/us/en/top3.html` にリダイレクトするように設定されています。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  experimental_redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## 関連Tutorials

[ トラフィックフィルタールールを使用した web サイトの保護 ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[HTTP ヘッダー検証 CDN ルールの設定とデプロイ ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[CDN キャッシュをパージする方法 ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[CDN エラーページの設定 ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[CDN でのトラフィックの設定 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[CDN 資格情報と認証の設定 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

