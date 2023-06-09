---
title: Adobe Analytics を使用した送信済みフォームデータフィールドに関するレポート
description: AEM Forms CS と Adobe Analytics を統合してフォームデータフィールドに関するレポートを作成する方法
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
exl-id: 369c563e-c847-438a-a783-bc6a9f81b77c
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '130'
ht-degree: 100%

---

# Adobe Analytics を使用した、フォームデータフィールド値とフォームフィールド検証エラーのレポート

タグと Adobe Analytics を使用してアダプティブフォームに分析を実装する方法について説明します。この例では、訪問者によるフォームの操作に関する洞察に満ちたレポートを生成するための設定および実装手順を順を追って説明します。

## 前提条件

このチュートリアルを最大限に活用するには、次の前提条件を満たすことをお勧めします。

* AEM Forms CS のある程度の使用経験
* Adobe Tags へのアクセス権限
* Adobe Analytics へのアクセス権限



このチュートリアルでは、AEM Forms に組み込まれているシンプルなアダプティブフォームを使用し、居住州の値のフォーム送信や、検証エラーが発生するフィールドを測定します。

![adaptive-form](assets/use-case.png)
