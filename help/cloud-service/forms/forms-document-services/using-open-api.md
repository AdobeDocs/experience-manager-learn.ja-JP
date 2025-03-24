---
title: AEM Forms Cummunication API の設定
description: サーバー間認証用の OpenAPI ベースの AEM Forms Communication API の設定
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: b9c0b04b-6131-4752-b2f0-58e1fb5f40aa
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 100%

---

# AEM Forms as a Cloud Service での AEM Forms Communication API の設定

## 前提条件

* AEM Forms as a Cloud Service の最新のインスタンス。
* 必要なすべての[製品プロファイルが環境に追加](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)されます。

* 以下に示すように、製品プロファイルへの AEM API アクセスを有効にします
  ![製品プロファイル 1](assets/product-profiles1.png)
  ![製品プロファイル](assets/product-profiles.png)

## Adobe Developer Console プロジェクトの作成

Adobe ID を使用して [Adobe Developer Console](https://developer.adobe.com/console/) にログインします。適切なアイコンをクリックして、新しいプロジェクトを作成します
![新規プロジェクト](assets/new-project.png)

プロジェクトに意味のある名前を付け、「API を追加」アイコンをクリックします
![新規プロジェクト](assets/new-project2.png)

Experience Cloud を選択します
![新規プロジェクト 3](assets/new-project3.png)
AEM Forms Communications API を選択して、「次へ」をクリックします
![新規プロジェクト 4](assets/new-project4.png)

サーバー間認証を選択していることを確認し、「次へ」をクリックします
![新規プロジェクト 5](assets/new-project5.png)
プロファイルを選択し、「設定済み API を保存」ボタンをクリックして設定を保存します
![新規プロジェクト 6](assets/new-project6.png)
OAuth サーバー間をクリックします
![新規プロジェクト 7](assets/new-project7.png)
クライアント ID、クライアント秘密鍵およびスコープをコピーします
![新規プロジェクト 8](assets/new-project8.png)

## ADC プロジェクト通信を有効にする AEM インスタンスの設定

AEM Forms プロジェクトが既にある場合は、[こちらの手順](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)に従って、Adobe Developer Console プロジェクトの OAuth サーバー間資格情報のクライアント IDを有効にして AEM インスタンスと通信できるようにします

AEM Forms プロジェクトがない場合は、[このドキュメントに従って AEM Forms プロジェクトを作成してください。](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started)次に、Adobe Developer Console プロジェクトの OAuth サーバー間資格情報のクライアント ID を有効にして、[こちらのドキュメントを使用](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)して AEM インスタンスと通信できるようにします。


## 次の手順

[アクセストークンの生成](./generate-access-token.md)
