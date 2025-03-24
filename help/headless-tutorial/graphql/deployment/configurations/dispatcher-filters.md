---
title: AEM GraphQL 用の Dispatcher フィルター
description: AEM GraphQL で使用する AEM パブリッシュ Dispatcher フィルターを設定する方法について説明します。
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10829
thumbnail: kt-10829.jpg
exl-id: b76b7c46-5cbd-4039-8fd6-9f0f10a4a84f
duration: 48
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 100%

---

# Dispatcher フィルター

Adobe Experience Manager as a Cloud Service は、AEM パブリッシュ Dispatcher フィルターを使用して、AEM に到達する必要のある要求のみが AEM に到達するようにします。デフォルトではすべてのリクエストが拒否され、許可された URL のパターンを明示的に追加する必要があります。

| クライアントタイプ | [シングルページアプリ（SPA）](../spa.md) | [Web コンポーネント／JS](../web-component.md) | [モバイル](../mobile.md) | [サーバー間](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Dispatcher フィルター設定が必要です | ✔ | ✔ | ✔ | ✔ |

>[!TIP]
>
> 設定例は次のようになります。プロジェクトの要件に合わせて調整してください。

## Dispatcher フィルター設定

AEM パブリッシュ Dispatcher フィルター設定は、AEM に到達するために許可される URL パターンを定義し、AEM で保持されるクエリエンドポイントの URL プレフィックスを含める必要があります。

| クライアントの接続先 | AEM オーサー | AEM パブリッシュ | AEM プレビュー |
|------------------------------------------:|:----------:|:-------------:|:-------------:|
| Dispatcher フィルター設定が必要です | ✘ | ✔ | ✔ |

`allow` ルールと URL パターン `/graphql/execute.json/*` を追加し、ファイル ID（例：`/0600` は、ファームファイルの例で一意であること）を確認します。
これにより、`HTTP GET /graphql/execute.json/wknd-shared/adventures-all` などの AEM パブリッシュに送信される永続化されたクエリエンドポイントに対する HTTP GET リクエストが可能になります。

AEM ヘッドレスエクスペリエンスでエクスペリエンスフラグメントを使用する場合は、これらのパスで同じ操作を行います。

+ `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
# Allow headless requests for Persisted Query endpoints
/0600 { /type "allow" /method '(POST|OPTIONS)' /url "/graphql/execute.json/*" }
# Allow headless requests for Experience Fragments
/0601 { /type "allow" /method '(GET|OPTIONS)' /url "/content/experience-fragments/*" }
...
```

### フィルター設定の例

+ [Dispatcher フィルターの例は、WKND プロジェクトにあります。](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.dispatcher.d/filters/filters.any#L28)
