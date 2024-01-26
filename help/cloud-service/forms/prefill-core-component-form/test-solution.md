---
title: コアコンポーネントベースのアダプティブフォームに事前入力する
description: アダプティブフォームにデータを事前入力する方法を学ぶ
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
duration: 28
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 3%

---

# ソリューションをテスト

コードをデプロイしたら、コアコンポーネントに基づいてアダプティブフォームを作成します。 以下のスクリーンショットに示すように、アダプティブフォームを事前入力サービスに関連付けます。
![prefill-service](assets/pre-fill-service.png)

フォームがレンダリングされるたびに、関連する事前入力サービスが実行され、事前入力サービスが返すデータがフォームに入力されます。

例えば、GUID に関連付けられたデータをフォームに事前入力する場合 **d815a2b3-5f4c-4422-8197-d0b73479bf0e**の場合、次の URL が使用されます。
事前入力サービスのコードは、guid パラメーターの値を抽出し、その guid に関連付けられたデータをデータソースから取得します。

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
