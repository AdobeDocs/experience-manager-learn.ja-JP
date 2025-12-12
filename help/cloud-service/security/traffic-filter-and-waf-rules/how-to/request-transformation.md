---
title: リクエストの標準化
description: AEM as a Cloud Service のトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
exl-id: eee81cd6-9090-45d6-b77f-a266de1d9826
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '259'
ht-degree: 100%

---

# リクエストの標準化

AEM as a Cloud Service のトラフィックフィルタールールを使用してリクエストを変換し、標準化する方法について説明します。

## リクエストを変換する理由とタイミング

リクエスト変換は、受信トラフィックを標準化し、不要なクエリパラメーターやヘッダーによって発生する不要な分散を減らす場合に役立ちます。この手法は、次の目的でよく使用されます。

- AEM アプリケーションに関連のないキャッシュバスティングパラメーターを削除することで、キャッシュの効率を向上させる。
- リクエストの並べ替えを最小限に抑え、不要な処理を軽減することで、オリジンを不正使用から保護する。
- リクエストを AEM に転送する前に、削除または簡素化する。

これらの変換は通常、CDN レイヤーで適用され、特にパブリックトラフィックを処理する AEM パブリッシュ層に適用されます。

## 前提条件

続行する前に、[トラフィックフィルタールールと WAF ルールの設定方法](../setup.md)チュートリアルの説明に従って必要な設定が完了していることを確認します。また、[AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd)のクローンを作成し、AEM 環境にデプロイしておきます。

## 例：アプリケーションで不要なクエリパラメーターの設定解除

この例では、キャッシュの断片化を減らすために、`search` と `campaignId` **以外のすべてのクエリパラメーターを削除**&#x200B;するルールを設定します。

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

- [以前に作成した](../setup.md#deploy-rules-using-adobe-cloud-manager) Cloud Manager 設定パイプラインを使用して、変更を AEM 環境にデプロイします。

- WKND サイトのページ（例：`https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`）にアクセスして、ルールをテストします。

- AEM ログ（`aemrequest.log`）では、リクエストが `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar` に変換され、`otherParam` が削除されていることが確認できます。

  ![WKND リクエスト変換](../assets/how-to/aemrequest-log-transformation.png)
