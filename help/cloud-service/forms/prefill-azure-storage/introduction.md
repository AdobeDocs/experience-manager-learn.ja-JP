---
title: アダプティブフォームの保存および再開機能の実装
description: アダプティブフォームデータを Azure ストレージアカウントに保存およびアカウントから取得する方法を説明します。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-13717
exl-id: b40b0ef4-efa9-400e-82d8-aa0c7feb7be4
badgeVersions: label="AEM FormsCloud Service" before-title="false"
duration: 28
source-git-commit: ed64dd303a384d48f76c9b8e8e925f5d3b8f3247
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 97%

---

# はじめに

このチュートリアルでは、フォーム入力者がフォーム入力プロセスを保存して再開できるようにする簡単なユースケースを実装します。このユースケースのフローは次のとおりです。

* ユーザーがアダプティブフォームの入力を開始します。
* ユーザーがフォームを保存し、後日フォームの入力を続行することを希望します。
* ユーザーが、保存したフォームへのリンクが記載されたメール通知を受信します。

## 前提条件

* AEM Forms CS、特にフォームデータモデル作成の経験。
* Cloud Manager を使用してコードをデプロイした経験。
* AEM Forms CS のクラウド対応インスタンスへのアクセス権限。

上記のユースケースを AEM Forms CS で実装するには、以下が必要です。

* [AEM Forms CS のクラウド対応インスタンス](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-and-aem-sync.html?lang=ja#set-up-aem-author-instance)
* [Azure ポータルアカウント](https://portal.azure.com/)
* [SendGrid アカウント](https://sendgrid.com/)

### 次の手順

[ページコンポーネントの作成](./page-component.md)
