---
title: Chatbot でのAEM Formsの使用
description: ChatBot データの解析
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
exl-id: 3c304b0a-33f8-49ed-a576-883df4759076
duration: 22
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# ChatBot データの解析

[ChatBot webhook](https://www.chatbot.com/help/webhooks/what-are-webhooks/) を使用して、ChatBot データをAEM サーブレットに送信しました。
ChatBot でキャプチャされたデータは JSON 形式で、以下のように attributes オブジェクトにユーザーが入力したデータを含んでいます
![ チャットボットデータ ](assets/chat-bot-data.png)

データを XDP テンプレートと結合するには、次の XML を作成する必要があります。 xml のルート要素に注意してください。データが正常に結合されるには、これは XDP テンプレートのルート要素と一致する必要があります。


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
