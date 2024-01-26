---
title: カスタム送信から応答を抽出
description: フォームの送信が正常に完了したときにカスタム応答を抽出する
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: e5f76d6a-2ea8-4909-9cfb-b673077cf8fd
duration: 16
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '51'
ht-degree: 100%

---

# 応答から json オブジェクトを抽出します。

ヘッドレスアダプティブフォームをカスタム送信ハンドラーに送信すると、送信ハンドラーによって返されたデータを抽出して React アプリに表示できます。 次のコードスニペットは、

```javascript
// associate event handler for the onSubmitSuccess event
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
// extract the json returned by the custom submit service
const onSuccess=(action) =>{
        let body = action.payload?.body;
        let FirstName = JSON.parse(body.metadata.json).firstName;
        setThankYouMessage(FirstName+" your request has been received");
        
      }
```
