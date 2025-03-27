---
title: コードプロジェクトの作成
description: ユニバーサルエディターを使用して編集できる、Edge Delivery Services のコードプロジェクトを作成します。
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '287'
ht-degree: 100%

---

# Edge Delivery Services コードプロジェクトの作成

Edge Delivery Services およびユニバーサルエディター用の AEM web サイトを作成するには、アドビの [AEM ボイラープレート XWalk プロジェクトテンプレート](https://github.com/adobe-rnd/aem-boilerplate-xwalk)を使用します。このテンプレートは、web サイトのエクスペリエンスを作成するのに使用される CSS と JavaScript を含む新しいコードプロジェクトを作成します。このテンプレートは、新しい GitHub リポジトリを作成し、アドビのボイラープレートコードと設定を読み込み、AEM web サイトプロジェクトの強固な基盤を提供します。

[Edge Delivery Services によって配信される AEM web サイト](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/sites/edge-delivery-services/overview)には、クライアントサイド（ブラウザー）コードのみが含まれます。Web サイトコードは、AEM オーサーサービスまたはパブリッシュサービスでは実行されません。

![新しい Edge Delivery Services プロジェクト](./assets/1-new-project/new-project.png)

ユニバーサルエディターでコンテンツを編集できる Edge Delivery Services コードプロジェクトを作成するには、[ドキュメントに記載された詳細な手順](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project)に従います。以下は、このチュートリアルで使用される値を含む手順の概要リストです。

1. **GitHub アカウントを設定します。**&#x200B;組織のプロジェクトを作成する場合は、組織に GitHub アカウントがあり、自分がメンバーであることを確認します。
2. [AEM ボイラープレート XWalk プロジェクトテンプレート](https://github.com/adobe-rnd/aem-boilerplate-xwalk)を使用して&#x200B;**新しいコードプロジェクトを作成します**。
3. **AEM Code Sync GitHub アプリをインストール**&#x200B;し、リポジトリへのアクセス権を付与します。アプリについて詳しくは、[こちら](https://github.com/apps/aem-code-sync)を参照してください。
4. **新しいプロジェクトの`fstab.yaml`** を正しい AEM オーサーサービスを指すように設定します。

   * 実験するには、下位の AEM as a Cloud Service 環境（ステージまたは開発）を使用できますが、実際の web サイトの実装では、実稼動の AEM サービスを使用するように設定する必要があります。

5. **新しいプロジェクトの`paths.json`** を編集して、AEM オーサーサービスのパスを web サイトのルートにマッピングします。

この Git リポジトリは、[ローカル開発環境](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment)の章でクローンが作成されます。コードはここで開発されます。
