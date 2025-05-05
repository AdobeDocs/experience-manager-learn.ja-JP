---
title: バッチデータ設定の指定
description: バッチデータ設定の指定
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Output Service
topic: Development
jira: KT-9673
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
duration: 233
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '141'
ht-degree: 100%

---

# バッチ設定の作成

バッチ API を使用するには、バッチ設定を作成し、その設定に基づいてバッチを実行します。次のビデオでは、API を使用してバッチ設定を作成するデモを示しています。

>[!NOTE]
>API 呼び出しを行うには、AEM ユーザーが ```forms-users``` グループに属していることを確認してください。


>[!VIDEO](https://video.tv.adobe.com/v/343701?quality=12&learn=on&captions=jpn)

## バッチ設定の作成

バッチ設定を作成するための POST エンドポイントを以下に示します。

```xml
<baseURL>/config
```

バッチ設定を作成する際に指定する必要がある最小限の設定は、次のとおりです。 これは、JSON オブジェクトとして HTTP リクエストの本文に渡す必要があります。

```
{
    "configName": "monthlystatements",
    "dataSourceConfigUri": "/conf/batchapi/settings/forms/usc/batch/batchapitutorial",
    "outputTypes": [
        "PDF"
    ],
    "template": "crx:///content/dam/formsanddocuments/formtemplates/custom_fonts.xdp"

}
```

## バッチ設定の確認

バッチ設定が正常に作成されたことを確認するには、次のエンドポイントに対して GET リクエストを呼び出します。


```xml
<baseURL>/config/monthlystatements
```

HTTP リクエストの本文に空の JSON オブジェクトを渡すだけです。
