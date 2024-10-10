---
title: フォームの添付ファイルをメールで送信します。
description: Power Automate ワークフローを使用して、送信されたフォームの添付ファイルを抽出してメールで送信します。
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '72'
ht-degree: 100%

---

# 送信されたフォームデータからフォームの添付ファイルを抽出します。

Power Automate ワークフローを使用して、フォームの添付ファイルを抽出し、メールで送信します。
次の動画では、送信されたデータからフォームの添付ファイルを抽出するために必要な手順を説明しています。
>[!VIDEO](https://video.tv.adobe.com/v/3409017?quality=12&learn=on)

Parse JSON スキーマのステップで使用する必要がある添付ファイルオブジェクトのスキーマは次のとおりです。

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
