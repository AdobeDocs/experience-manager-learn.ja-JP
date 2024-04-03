---
title: アダプティブフォームへの SharePoint リストのデータの事前入力
description: SharePoint リストに基づくフォームデータモデルを使用してアダプティブフォームに事前入力する方法を説明します。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
duration: 60
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: 3dc1aea74e2a7cf30da9f6fb96ecc5c7edcf6e34
workflow-type: ht
source-wordcount: '234'
ht-degree: 100%

---

# アダプティブフォームへの SharePoint リストデータの事前入力

AEM Forms の以前のバージョン（6.5）では、request 属性を使用して、フォームデータモデルベースのアダプティブフォームに事前に入力するためのカスタムコードを記述する必要がありました。 AEM Forms as a Cloud Service では、カスタムコードを記述する必要がなくなりました。

この記事では、フォームデータモデルの事前入力サービスを使用して、SharePoint リストから取得したデータをアダプティブフォームに事前入力する手順について説明します。

この記事は、[SharePoint リストにデータを送信するようにアダプティブフォームが正常に設定されている](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=ja#connect-af-sharepoint-list)ことを前提としています。

次に、SharePoint リストのデータを示します。
![sharepoint-list](assets/list-data.png)

特定の GUID に関連付けられたデータをアダプティブフォームに事前入力するには、次の手順を実行する必要があります

## GET サービスの設定

* guid 属性を使用して、フォームデータモデルの最上位オブジェクトの GET サービスを作成する
  ![get-service](assets/mapping-request-attribute.png)

このスクリーンショットでは、guid 列は、`submissionid` という名前のリクエスト属性を使用して連結されています。

GET サービスが完全に設定されると、次のようになります

![get-service](assets/fdm-request-attribute.png)

## フォームデータモデルの事前入力サービスを使用するようにアダプティブフォームを設定する

* SharePoint リストフォームのデータモデルに基づいたアダプティブフォームを開きます。フォームデータモデルの事前入力サービスの関連付け
  ![form-prefill-service](assets/form-prefill-service.png)

## フォームのテスト

以下に示すように、URL に `submissionid` を含めてフォームをプレビューします。

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```
