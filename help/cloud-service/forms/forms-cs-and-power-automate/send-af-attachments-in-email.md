---
title: フォームの添付ファイルをメールで送信します。
description: Power Automate ワークフローを使用して、送信されたフォームの添付ファイルを抽出してメールで送信します。
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-11077
exl-id: 1be90d9b-3669-44a0-84fb-cbdec44074d8
duration: 391
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '78'
ht-degree: 100%

---

# 送信されたフォームデータからフォームの添付ファイルを抽出します。

Power Automate ワークフローを使用して、フォームの添付ファイルを抽出し、メールで送信します。
次の動画では、送信されたデータからフォームの添付ファイルを抽出するために必要な手順を説明しています。
>[!VIDEO](https://video.tv.adobe.com/v/3413030?quality=12&learn=on&captions=jpn)

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
