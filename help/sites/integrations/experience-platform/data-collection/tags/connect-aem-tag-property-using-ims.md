---
title: IMS を使用した AEM Sites とタグプロパティの接続
description: AEMでの IMS 設定を使用して、AEM Sitesをタグプロパティに接続する方法について説明します。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 72
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 40%

---

# IMS を使用した AEM Sites とタグプロパティの接続{#connect-aem-and-tag-property-using-ims}

AEMの IMS(Identity Management System) 設定を使用して、AEMをタグプロパティに接続する方法を説明します。 この設定では、タグ API を使用してAEMを認証し、AEMがタグ API を介して通信してタグのプロパティにアクセスできるようにします。

## IMS 設定の作成または再利用

AEM を新規作成のタグプロパティと統合するには、Adobe Developer Console プロジェクトを使用した IMS 設定が必要です。この設定により、AEMはタグ API を使用してタグアプリケーションと通信でき、IMS はこの統合のセキュリティ側面を処理します。

AEM as aCloud Service環境がプロビジョニングされるたびに、Asset compute、Adobe Analytics、タグなど、いくつかの IMS 設定が自動的に作成されます。 自動作成された **Adobe Experience Platformのタグ** IMS 設定を使用できます。または、AEM 6.X 環境を使用している場合は、新しい IMS 設定を作成する必要があります。

自動作成されたレビュー **Adobe Experience Platformのタグ** 次の手順を使用した IMS の設定

1. AEM Author で、 **ツール** メニュー
1. 「セキュリティ」セクションで、Adobe IMS 設定を選択します。
1. **Adobe Launch** カードを選択し、「**プロパティ**」をクリックし、「**証明書**」タブと「**アカウント**」タブで詳細を確認します。次に、「**キャンセル**」をクリックして、自動作成された詳細を変更せずに戻ります。
1. **Adobe Launch** カードを選択し、今度は「**正常性をチェック**」をクリックします。以下のような&#x200B;**成功**&#x200B;メッセージが表示されます。

   ![タグの正常な IMS 設定](assets/adobe-launch-healthy-ims-config.png)

## 次の手順

[AEMでタグCloud Service設定を作成する](create-aem-launch-cloud-service.md)
