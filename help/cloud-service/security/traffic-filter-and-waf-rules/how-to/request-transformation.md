---
title: リクエストの標準化
description: AEM as a Cloud Serviceのトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法を説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 8%

---

# リクエストの標準化

AEM as a Cloud Serviceのトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法を説明します。

## リクエストを変換する理由とタイミング

リクエスト変換は、受信トラフィックを正規化し、不要なクエリパラメーターやヘッダーによって発生する不要な分散を減らす場合に便利です。 この手法は、以下の目的でよく使用されます。

- AEM アプリケーションに関係のないキャッシュバスティングパラメーターを削除することで、キャッシュの効率を向上させます。
- リクエストの並べ替えを最小限に抑え、不要な処理を軽減することで、オリジンを不正使用から保護します。
- リクエストがAEMに転送される前に、そのリクエストの不要部分を削除したりシンプル化したりする。

これらの変換は、通常、特に、パブリックトラフィックを扱うAEM パブリッシュ層の CDN レイヤーで適用されます。

## 前提条件

続行する前に、[ トラフィックフィルターとWAF ルールの設定方法 ](../setup.md) チュートリアルの説明に従って、必要な設定を完了していることを確認してください。 また、[AEM WKND サイトプロジェクト ](https://github.com/adobe/aem-guides-wknd) のクローンを作成して、AEM環境にデプロイしました。

## 例：アプリケーションで不要なクエリパラメーターの設定解除

この例では、キャッシュの断片化を減らすために **を除くすべてのクエリパラメーターを削除**`search` および `campaignId` というルールを設定します。

- 次のルールを WKND プロジェクトの `/config/cdn.yaml` ファイルに追加します。

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- 変更をコミットして Cloud Manager Git リポジトリにプッシュします。

- Cloud Manager設定パイプラインを使用して [ 以前に作成した ](../setup.md#deploy-rules-using-adobe-cloud-manager) 変更内容をAEM環境にデプロイします。

- WKND サイトのページにアクセスして、ルールをテストします（例：`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`）。

- AEMのログ（`aemrequest.log`）で、リクエストが `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar` に変換され、`otherParam` が削除されていることがわかります。

  ![WKND リクエスト変換 ](../assets/how-to/aemrequest-log-transformation.png)

