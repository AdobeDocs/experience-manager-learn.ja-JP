---
title: チャットボットでの AEM Forms の使用
description: チャットボットデータの解析
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '97'
ht-degree: 100%

---

# チャットボットデータの解析

チャットボットデータを AEM サーブレットに送信するために、[チャットボット web フック](https://www.chatbot.com/help/webhooks/what-are-webhooks/)が使用されました。
チャットボットで取得されたデータは、次に示すように、ユーザーが属性オブジェクトにデータを入力した JSON 形式です
![チャットボットデータ](assets/chat-bot-data.png)

データを XDP テンプレートと結合するには、次の XML を作成する必要があります。xml のルート要素に注意してください。データを正常に結合するには、これは XDP テンプレートのルート要素と一致する必要があります。


```xml
<topmostSubForm>
    <f1_01>David Smith</f1_01>
    <signmethod>ESIGN</signmethod>
    <corporation>1</corporation>
    <f1_08>San Jose, CA, 95110</f1_08>
    <f1_07>345 Park Avenue</f1_07>
    <ssn>123-45-6789</ssn>
    <form_name>W-9</form_name>
</topmostSubForm>
```

![xdp-template](assets/xdp-template.png)

## 次の手順

[データと XDP テンプレートの結合](./merge-data-with-template.md)
