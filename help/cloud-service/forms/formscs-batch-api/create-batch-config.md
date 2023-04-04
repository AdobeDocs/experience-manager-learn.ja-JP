---
title: バッチデータ設定の指定
description: バッチデータ設定の指定
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9673
exl-id: db25e5a2-e1a8-40ad-af97-35604d515450
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 12%

---

# バッチ設定を作成

バッチ API を使用するには、バッチ設定を作成し、その設定に基づいてバッチを実行します。次のビデオは、API を使用したバッチ設定の作成のデモを示しています

>[!NOTE]
>AEMユーザーがに属していることを確認してください ```forms-users``` グループを使用して API 呼び出しをおこないます。


>[!VIDEO](https://video.tv.adobe.com/v/340241?quality=12&learn=on)

## バッチ設定を作成

次に、バッチ設定を作成するPOSTエンドポイントを示します。

```xml
<baseURL>/config
```

バッチ設定を作成する際に指定する必要がある最小の設定は、次のとおりです。 これは、HTTP リクエストの本文で JSON オブジェクトとして渡す必要があります

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

## バッチ設定を検証

バッチ設定が正常に作成されたことを確認するには、次のエンドポイントに対してGETリクエストを呼び出します


```xml
<baseURL>/config/monthlystatements
```

必要なのは、HTTP リクエストの本文で空の JSON オブジェクトを渡すだけです
