---
title: Chatbot でのAEM Formsの使用
description: ChatBot データの解析
feature: Adaptive Forms
version: 6.5
jira: KT-15344
topic: Development
role: User
level: Intermediate
source-git-commit: eb4463ae0270725c5b0bd96e9799bada25b06303
workflow-type: tm+mt
source-wordcount: '97'
ht-degree: 2%

---

# ChatBot データの解析

A [ChatBot webhook](https://www.chatbot.com/help/webhooks/what-are-webhooks/) は、ChatBot データをAEM サーブレットに送信するために使用されました。
ChatBot でキャプチャされたデータは JSON 形式で、以下のように attributes オブジェクトにユーザーが入力したデータを含んでいます
![chatbot-data](assets/chat-bot-data.png)

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

![xdp テンプレート](assets/xdp-template.png)

## 次の手順

[データと XDP テンプレートの結合](./merge-data-with-template.md)



