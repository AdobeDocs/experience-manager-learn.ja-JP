---
title: コアコンポーネントベースのアダプティブフォームに事前入力する
description: アダプティブフォームにデータを事前入力する方法を学ぶ
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14675
duration: 23
exl-id: a94deebd-e86e-4360-b0ed-193f13197ee2
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 100%

---

# ソリューションのテスト

コードをデプロイしたら、コアコンポーネントに基づいてアダプティブフォームを作成します。以下のスクリーンショットに示すように、アダプティブフォームを事前入力サービスに関連付けます。
![prefill-service](assets/pre-fill-service.png)

フォームがレンダリングされるたびに、関連する事前入力サービスが実行され、事前入力サービスが返すデータがフォームに入力されます。

例えば、GUID に関連付けられたデータをフォームに事前入力する&#x200B;**d815a2b3-5f4c-4422-8197-d0b73479bf0e**の場合、次の URL が使用されます。
事前入力サービスのコードは、GUID パラメーターの値を抽出し、その GUID に関連付けられたデータをデータソースから取得します。

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
