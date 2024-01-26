---
title: 送信されたデータを確認するワークフローの作成
description: AEM Forms ワークフローコンポーネントを使用して AEM ワークフローモデルを作成し、送信されたデータを確認します。
feature: Workflow
doc-type: article
version: 6.4,6.5
jira: KT-4271
thumbnail: 40242.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0aafd97a-2e72-4257-ad4d-e0993263b11f
last-substantial-update: 2020-07-07T00:00:00Z
duration: 1053
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '127'
ht-degree: 100%

---

# 送信されたデータを確認するワークフローの作成

ワークフローは通常、送信されたデータをレビューと承認のためにルーティングする際に使用されます。ワークフローは、AEM のワークフローエディターを使用して作成されます。このワークフローは、アダプティブフォームの送信時にトリガーできます。

## 前提条件

AEM Forms のインスタンスが機能していることを確認してください。[インストールガイド](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html?lang=ja)に従って、AEM Forms をインストールおよび設定してください。

次のビデオでは、AEM Forms コンポーネントを使用したレビューと承認のワークフローの作成について説明しています。
>[!VIDEO](https://video.tv.adobe.com/v/40242?quality=12&learn=on)


何らかの理由でワークフローを作成できない場合は、完成したワークフローを[こちら](assets/review-submitted-data-workflow.zip)からダウンロードし、[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して AEM インスタンスに読み込むことができます。
