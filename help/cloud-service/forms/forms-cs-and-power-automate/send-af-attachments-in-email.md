---
title: フォームの添付ファイルを電子メールで送信する
description: Power Automate Workflow を使用して、送信されたフォームの添付ファイルを E メールで抽出して送信
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 11077
source-git-commit: aea43a705b3959f8be26238d32b816b3953e153e
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 送信されたフォームデータからフォームの添付ファイルを抽出する

フォームの添付ファイルを抽出し、power automate ワークフローで添付ファイルを電子メールで送信します。
次のビデオでは、送信されたデータから添付ファイルを作成するために必要な手順を説明します。
>[!VIDEO](https://video.tv.adobe.com/v/3409017/?quality=12&learn=on)

次に、「 Parse JSON schema 」手順で使用する必要がある添付ファイルオブジェクトスキーマを示します

```json
{
    "type": "object",
    "properties": {
        "filename": {
            "type": "string"
        },
        "data": {
            "type": "string"
        },
        "contentType": {
            "type": "string"
        },
        "size": {
            "type": "integer"
        }
    }
}
```
