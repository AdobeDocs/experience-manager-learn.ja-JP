---
title: AEM Forms と Adobe Analytics を統合してフォームデータフィールドに関するレポートを作成する方法
description: AEM Forms as a Cloud Service と Adobe Analytics を統合してフォームデータフィールドに関するレポートを作成する方法
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 369c563e-c847-438a-a783-bc6a9f81b77c
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '157'
ht-degree: 100%

---

# AEM Forms と Adobe Analytics を統合してフォームデータフィールドに関するレポートを作成する方法

Experience Platform タグを使用してアダプティブフォーム上で AEM Forms as a Cloud Service と Adobe Analytics を統合する方法を説明します。この例では、訪問者によるフォームの操作に関する洞察に満ちたレポートを生成するための設定および実装手順を順を追って説明します。

## 前提条件

このチュートリアルを最大限に活用するには、次の前提条件を満たすことをお勧めします。

* AEM Forms as a Cloud Service の使用経験
* Experience Platform タグへのアクセス権限
* Adobe Analytics へのアクセス権限

このチュートリアルでは、AEM Forms に組み込まれているシンプルなアダプティブフォームを使用し、居住州の値のフォーム送信や、検証エラーが発生するフィールドを測定します。

![adaptive-form](assets/use-case.png)

## 次の手順

[データ要素の作成](./data-elements.md)