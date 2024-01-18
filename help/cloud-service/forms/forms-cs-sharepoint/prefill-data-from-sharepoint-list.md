---
title: SharePoint リストのデータを使用してアダプティブフォームに事前入力する
description: 共有ポイントリストに基づくフォームデータモデルを使用してアダプティブフォームに事前入力する方法を説明します。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
source-git-commit: aa6cd605c617de45003a96b1c14e37f055a8c566
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# 共有ポイントリストデータを使用してアダプティブフォームに事前入力する

AEM Forms(6.5) の以前のバージョンでは、request 属性を使用して、フォームデータモデルベースのアダプティブフォームに事前に入力するためのカスタムコードを記述する必要がありました。 AEM Forms as a cloud service では、カスタムコードを記述する必要がなくなりました。

この記事では、フォームデータモデルの事前入力サービスを使用して、SharePoint リストから取得したデータをアダプティブフォームに事前入力する手順について説明します。

この記事は、 [データを sharepoint リストに送信するようアダプティブフォームが正常に設定されました。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)

次に、SharePoint リストのデータを示します。
![sharepoint-list](assets/list-data.png)

特定の GUID に関連付けられたデータをアダプティブフォームに事前入力するには、次の手順を実行する必要があります

## get サービスを設定する

* guid 属性を使用して、フォームデータモデルの最上位オブジェクトの get サービスを作成する
  ![get-service](assets/mapping-request-attribute.png)

このスクリーンショットでは、guid 列は、という名前のリクエスト属性を使用して連結されています。 `submissionid`.

get サービスが完全に設定されると、次のようになります

![get-service](assets/fdm-request-attribute.png)

## フォームデータモデルの事前入力サービスを使用するようにアダプティブフォームを設定する

* 共有ポイントリストフォームのデータモデルに基づいたアダプティブフォームを開きます。 フォームデータモデル事前入力サービスを関連付ける
  ![form-prefill-service](assets/form-prefill-service.png)

## フォームをテストする

次を含めてフォームをプレビューします。 `submissionid` 」と入力します。

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```




