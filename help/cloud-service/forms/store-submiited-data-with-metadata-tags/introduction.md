---
title: ユーザー指定のメタデータタグを追加するチュートリアル
description: アダプティブフォームデータを Azure ストレージアカウントに保存する方法およびアカウントから取得する方法を学びます。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14501
duration: 24
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a8531e82-18cd-4b32-8148-d6fc5f6e51c6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '136'
ht-degree: 100%

---

# はじめに

このチュートリアルでは、Blob インデックスタグを使用して Azure ストレージにフォーム送信を保存する、簡単なユースケースの実装方法を学びます。Blob インデックスタグは、キーと値のインデックスタグ属性を使用して、データの管理と検出の機能を提供するものです。単一のコンテナ内、またはストレージアカウント内のすべてのコンテナに対して、オブジェクトを分類して検索できます。
![blob-index-tags](assets/blob-with-index-tags.png)

## 前提条件

* AEM Forms CS でのエクスペリエンス。
* Cloud Manager を使用してコードをデプロイした経験。
* AEM Forms CS のクラウド対応インスタンスへのアクセス権限。

上記のユースケースを AEM Forms CS で実装するには、以下が必要です。

* [AEM Forms CS のクラウド対応インスタンス](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=ja#set-up-aem-author-instance)
* [Azure ポータルアカウント](https://portal.azure.com/)


### 次の手順

[extend-choice-group-components](./extend-choice-group-components.md)
