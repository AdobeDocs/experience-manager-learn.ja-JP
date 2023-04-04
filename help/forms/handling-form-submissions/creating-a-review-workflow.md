---
title: 送信されたデータを確認するワークフローの作成
description: AEM Formsワークフローコンポーネントを使用したAEM Workflow モデルの作成で、送信されたデータを確認します。
feature: Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
kt: 4271
thumbnail: 40242.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0aafd97a-2e72-4257-ad4d-e0993263b11f
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 4%

---

# 送信されたデータを確認するワークフローの作成

通常、ワークフローは、レビューと承認のために送信されたデータをルーティングするために使用されます。 ワークフローは、AEMのワークフローエディターを使用して作成されます。 このワークフローは、アダプティブフォームの送信時にトリガーできます。

## 前提条件

AEM Formsのインスタンスが正常に動作していることを確認してください。 詳しくは、 [インストールガイド](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) AEM Formsをインストールして設定する

次のビデオでは、 AEM Formsコンポーネントを使用したレビューと承認ワークフローの作成について説明します
>[!VIDEO](https://video.tv.adobe.com/v/40242?quality=12&learn=on)


何らかの理由でワークフローを構築できない場合は、完了したワークフローを [ここ](assets/review-submitted-data-workflow.zip) を使用して同じものを読み込みます。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp) をAEMインスタンスに追加します。
