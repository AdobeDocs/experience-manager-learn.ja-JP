---
title: AEM GraphQL の Dispatcher フィルター
description: AEM GraphQL で使用する AEM パブリッシュ Dispatcher フィルターを設定する方法について説明します。
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10829
thumbnail: kt-10829.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 2%

---


# Dispatcher フィルター

Adobe Experience Manager as a Cloud Serviceは、AEMに到達する必要のある要求のみがAEMに到達するように、AEM パブリッシュ Dispatcher フィルターを使用します。 デフォルトでは、すべての要求が拒否され、許可された URL のパターンを明示的に追加する必要があります。

| クライアントタイプ | [シングルページアプリ (SPA)](../spa.md) | [Web コンポーネント/JS](../web-component.md) | [モバイル](../mobile.md) | [サーバー間](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Dispatcher フィルターの設定が必要です | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 次の設定は例です。 プロジェクトの要件に合わせて調整してください。

## Dispatcher のフィルター設定

AEM パブリッシュ Dispatcher フィルターの設定は、AEMに到達するのに許可される URL パターンを定義し、AEMで保持されるクエリエンドポイントの URL プレフィックスを含める必要があります。

| クライアントの接続先 | コンテンツ作成者 | AEM パブリッシュ | AEMプレビュー |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher フィルターの設定が必要です | ✘ | ✔ | ✔ |

を追加します。 `allow` ルールと URL パターン `/graphql/execute.json/*`をクリックし、ファイル ID( 例： `/0600`は、サンプルのファームファイルで一意です )。
これにより、永続化されたクエリエンドポイント（例： ）に対する HTTPGETリクエストが可能になります。 `HTTP GET /graphql/execute.json/wknd-shared/adventures-all` AEM パブリッシュに切り替えます。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0600 { /type "allow" /url "/graphql/execute.json/*" }
...
```

### フィルター設定の例

+ [Dispatcher フィルターの例は、WKND プロジェクトに記載されています。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)