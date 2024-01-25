---
title: ユーザー指定のメタデータタグを追加するためのチュートリアル
description: アダプティブフォームデータを Azure ストレージアカウントに保存およびアカウントから取得する方法を説明します。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 27
exl-id: a8531e82-18cd-4b32-8148-d6fc5f6e51c6
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 45%

---

# はじめに

このチュートリアルでは、BLOB インデックスタグを使用して Azure ストレージにフォーム送信を保存する簡単な使用例を実装する方法を学びます。 BLOB インデックスタグは、キー値インデックスタグ属性を使用して、データの管理と検出の機能を提供します。 単一のコンテナ内、またはストレージアカウント内のすべてのコンテナ間で、オブジェクトを分類して検索できます。
![blob-index-tags](assets/blob-with-index-tags.png)

## 前提条件

* AEM Forms CS を使った経験
* Cloud Manager を使用してコードをデプロイした経験。
* AEM Forms CS のクラウド対応インスタンスへのアクセス権限。

上記のユースケースを AEM Forms CS で実装するには、以下が必要です。

* [AEM Forms CS のクラウド対応インスタンス](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=ja#set-up-aem-author-instance)
* [Azure ポータルアカウント](https://portal.azure.com/)


### 次の手順

[extend-choice-group-components](./extend-choice-group-components.md)
