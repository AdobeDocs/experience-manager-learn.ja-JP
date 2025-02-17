---
title: カスタム送信サービスにヘッドレスフォームを送信する
description: 送信されたデータに基づいて応答をカスタマイズする
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 78fe677c-d5ab-40f6-a381-800f24e227ae
duration: 27
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: ht
source-wordcount: '141'
ht-degree: 100%

---

# 送信されたデータに基づいて応答をカスタマイズ

フォームが送信された後、送信の結果に関するフィードバックをユーザーに提供することが重要です。 送信応答には、トランザクション ID を含めることも、単にパーソナライズされた応答を含めることもできます。 この使用例を満たすために、カスタム送信サービスがAEM Formsで記述され、ヘッドレスフォームがこのカスタム送信サービスに送信されます。

## 前提条件

この機能を正しく実装するには、次の点に関する知識をお勧めします

* Git の使用経験
* AEM Cloud Manager を使用した経験
* Maven（この記事は 3.8.6 でテストされました）
* ローカルのAEM Forms Cloud 対応オーサーインスタンス
* AEM Forms as aCloud Service環境へのアクセス
* IntelliJ またはその他の IDE


## 次の手順

[カスタム送信サービスの書き込み](./custom-submit-service.md)
