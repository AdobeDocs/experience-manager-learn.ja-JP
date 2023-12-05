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
duration: 40
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '128'
ht-degree: 46%

---

# はじめに

このチュートリアルでは、BLOB インデックスタグを使用して Azure ストレージにフォーム送信を保存する簡単な使用例を実装します。 BLOB インデックスタグは、キー値インデックスタグ属性を使用して、データの管理と検出の機能を提供します。 単一のコンテナ内、またはストレージアカウント内のすべてのコンテナ間で、オブジェクトを分類して検索できます。
![blob-index-tags](assets/blob-with-index-tags.png)

## 前提条件

* AEM Forms CS を使用した経験がある。
* Cloud Manager を使用してコードをデプロイした経験。
* AEM Forms CS のクラウド対応インスタンスへのアクセス権限。

上記のユースケースを AEM Forms CS で実装するには、以下が必要です。

* [AEM Forms CS のクラウド対応インスタンス](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=ja#set-up-aem-author-instance)
* [Azure ポータルアカウント](https://portal.azure.com/)


### 次の手順

[extend-choice-group-components](./extend-choice-group-components.md)
