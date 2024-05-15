---
title: バッチ設定の実行
description: バッチを実行して、ドキュメント生成プロセスを起動します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
duration: 219
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 100%

---

# バッチ設定の実行

バッチを実行するには、次の API に対して POST リクエストを行います。

```xml
<baseURL>/confi/<configName>/execution
```

この API では、リクエスト本文のパラメーターとして空の JSON オブジェクトが必要です。
この API は、**location** キーによって特定された応答ヘッダー内の一意の URL を返します。
この一意の URL に対して GET リクエストを行うと、バッチ実行のステータスが返されます。

次のビデオでは、バッチ設定のトリガーについて説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
