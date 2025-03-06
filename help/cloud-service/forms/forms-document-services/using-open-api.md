---
title: AEM Forms通信 API の設定
description: サーバー間認証のための OpenAPI ベースのAEM Forms Communication API の設定
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 10%

---

# AEM Forms as a Cloud Serviceで OpenAPI ベースのAEM Forms通信 API を設定する

## 前提条件

* AEM Forms as a Cloud Serviceの最新のインスタンス。
* 必要なすべての [ 製品プロファイルが環境に追加されます ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)。

* 以下に示すように、製品プロファイルへのAEM API アクセスを有効にします
  ![product_profile1](assets/product-profiles1.png)
  ![product_profile](assets/product-profiles.png)

## Adobe Developer Console プロジェクトの作成

Adobe IDを使用して ](https://developer.adobe.com/console/)0}Adobe Developer Console} にログインします。
[適切なアイコンをクリックして、新しいプロジェクトを作成します
![ 新規プロジェクト ](assets/new-project.png)

プロジェクトに意味のある名前を付け、「API を追加」アイコンをクリックします。
![ 新規プロジェクト ](assets/new-project2.png)

Experience Cloudを選択
![new-project3](assets/new-project3.png)
AEM Forms Communications API を選択して、「次へ」をクリックします。
![new-project4](assets/new-project4.png)

サーバー間認証を選択していることを確認し、「次へ」をクリックします
![new-project5](assets/new-project5.png)
プロファイルを選択し、「設定済み API を保存」ボタンをクリックして設定を保存します。
![new-project6](assets/new-project6.png)
OAuth サーバー間をクリック
![new-project7](assets/new-project7.png)
クライアント ID、クライアント秘密鍵、スコープをコピーします
![new-project8](assets/new-project8.png)

## ADC プロジェクト通信を有効にする AEM インスタンスの設定

既にAEM Forms プロジェクトがある場合は [ 次の手順に従って ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)、Adobe Developer Console プロジェクトの OAuth サーバー間資格情報のクライアント ID がAEM インスタンスと通信できるようにします

AEM Forms プロジェクトがない場合は、このドキュメントに従って [AEM Forms プロジェクトを作成してください。](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/getting-started) 次に、Adobe Developer Console プロジェクトの OAuth サーバー間資格情報 ClientID を有効にして、（このドキュメントを使用して [AEM インスタンスと通信できるようにします。](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/aem-apis/invoke-openapi-based-aem-apis)


## 次の手順

[アクセストークンの生成](./generate-access-token.md)