---
title: アダプティブフォームの保存と再開機能の実装
description: Azure ストレージアカウントからアダプティブフォームデータを保存し、取得する方法について説明します。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 3%

---

# はじめに

このチュートリアルでは、フォーム入力者がフォーム入力プロセスを保存および再開できるようにする簡単な使用例を実装します。 このユースケースの流れは次のとおりです

* ユーザーがアダプティブフォームの入力を開始します。
* ユーザーがフォームを保存し、後日フォームの入力を続行します。
* ユーザーは、保存されたフォームへのリンクが記載された電子メール通知を受け取ります。

## 前提条件

* AEM Forms CS を使用して、特にフォームデータモデルの作成を体験できます。
* Cloud Manager を使用してコードをデプロイする経験があります。
* AEM Forms CS のクラウド対応インスタンスへのアクセス

AEM Forms CS で上記の使用例を実装するには、次が必要です

* [AEM Forms CS cloud Ready インスタンス](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=en#set-up-aem-author-instance)
* [Azure ポータルアカウント](https://portal.azure.com/)
* [SendGrid アカウント](https://sendgrid.com/)

### 次の手順

[ページコンポーネントを作成](./page-component.md)


