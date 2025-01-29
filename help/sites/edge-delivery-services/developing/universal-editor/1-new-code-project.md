---
title: コードプロジェクトの作成
description: ユニバーサルエディターを使用して編集可能な、Edge Delivery Services用のコードプロジェクトを作成します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 9b10d79190d805b86884f033e040891655c3c890
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---

# Edge Delivery Servicesコードプロジェクトの作成

Edge Delivery Servicesおよびユニバーサルエディター用のAEM web サイトを作成するには、Adobeの [AEM Boilerplate XWalk プロジェクトテンプレート ](https://github.com/adobe-rnd/aem-boilerplate-xwalk) を使用します。 このテンプレートは、web サイトエクスペリエンスの作成に使用される CSS とJavaScriptを含む新しいコードプロジェクトを作成します。 このテンプレートは、新しい GitHub リポジトリを作成し、Adobeのボイラープレートコードと設定を読み込み、AEM web サイトプロジェクトの強固な基盤を提供します。

Edge Delivery Servicesが配信する [AEM web サイトには ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/sites/edge-delivery-services/overview) クライアントサイド（ブラウザー）コードのみが含まれていることに注意してください。 Web サイトコードは、AEM オーサーサービスまたはPublish サービスでは実行されません。

![ 新規Edge Delivery Servicesプロジェクト ](./assets/1-new-project/new-project.png)

ユニバーサルエディターでコンテンツを編集可能なEdge Delivery Servicesコードプロジェクトを作成するには、次の手順に従います。

1. **GitHub アカウントを設定します。** 組織のプロジェクトを作成する場合は、組織に GitHub アカウントがあり、自分がメンバーであることを確認します。
2. **2}AEM Boilerplate XWalk プロジェクトテンプレートを使用して** 新しいコードプロジェクトを作成します ](https://github.com/adobe-rnd/aem-boilerplate-xwalk)。[
3. **AEM コード同期 GitHub アプリをインストール** し、リポジトリへのアクセス権を付与します。 [ アプリはこちら ](https://github.com/apps/aem-code-sync) で確認できます。
4. **新しいプロジェクトの`fstab.yaml`** を、正しいAEM オーサーサービスを指すように設定します。

   * 実験には、下位のAEM as a Cloud Service環境（ステージングまたは開発）を使用できますが、実際の web サイトの実装は実稼動のAEM サービスを使用するように設定する必要があります。

5. **新しいプロジェクトの`paths.json`** を編集して、AEM オーサーサービスのパスを web サイトのルートにマッピングします。

詳しい手順については、はじめる前に [GitHub プロジェクトの作成 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) の節を参照してください。
