---
title: バッチ設定の実行
description: バッチを実行してドキュメント生成プロセスを開始します
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 9674
exl-id: 17f91f81-96d8-49d6-b1e3-53d8899695ae
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '81'
ht-degree: 0%

---

# バッチ設定を実行

バッチを実行するには、次の API に対してPOSTリクエストを実行します

```xml
<baseURL>/confi/<configName>/execution
```

この API では、リクエスト本文のパラメーターとして空の json オブジェクトが必要です。
この API は、 **場所** キー。
この一意の URL に対するGETリクエストは、バッチ実行のステータスを示します

次のビデオでは、バッチ設定のトリガーを示します

>[!VIDEO](https://video.tv.adobe.com/v/340242?quality=12&learn=on)
