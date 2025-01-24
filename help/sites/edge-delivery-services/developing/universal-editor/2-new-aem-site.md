---
title: AEM サイトの作成
description: AEM Sitesで、ユニバーサルエディターを使用して編集可能なEdge Delivery Services用のサイトを作成します。
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
source-git-commit: e8ce91b0be577ec6cf8f3ab07ba9ff09c7e7a6ab
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# AEM サイトの作成

AEM サイトは、web サイトのコンテンツの編集、管理、公開を行う場所です。 Edge Delivery Servicesを介して配信され、ユニバーサルエディターを使用して作成されたAEM サイトを作成するには、[AEM オーサリングサイトテンプレートとのEdge Delivery Services](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) を使用して、AEM オーサー上に新しいサイトを作成します。

AEM サイトは、web サイトのコンテンツが保存およびオーサリングされる場所です。 最終的なエクスペリエンスは、AEM サイトのコンテンツと [web サイトのコードを組み合わせたもの ](./1-new-code-project.md) す。

![Edge Delivery Servicesとユニバーサルエディターの新しいAEM サイト ](./assets/2-new-aem-site/new-site.png)

新しいAEM サイトを作成するには、次の手順に従います。

1. AEM オーサーで **新しいサイトを作成** します。 このチュートリアルでは、次のサイト命名を使用します。
   * サイトのタイトル：`WKND (Universal Editor)`
   * サイト名：`aem-wknd-eds-ue`
2. **AEM オーサリングサイトテンプレートを使用して、[Edge Delivery Servicesから** 最新のテンプレートを読み込みます ](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases)。
3. GitHub リポジトリ名と一致するように **サイトに名前を付け**、「GitHub URL」をリポジトリの URL として設定します。

手順について詳しくは、はじめる前にの [ 新しいAEM サイトの作成と編集 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) の節を参照してください。

## プレビューする新しいサイトをPublish

AEM オーサーでサイトを作成した後、Edge Delivery Services プレビューに公開して、[ローカル開発環境でコンテンツを使用できるように ](./3-local-development-environment.md) ます。

1. **AEM オーサーにログインし****Sites** に移動します。
2. **新しいサイト** （`WKND (Universal Editor)`）を選択し、「**パブリケーションの管理**」をクリックします。
3. **宛先** の下の **プレビュー** を選択し、**次へ** をクリックします。
4. **子を含める設定** で、「**子を含める**」を選択し、他のオプションの選択を解除して、「**OK**」をクリックします。
5. **Publish** をクリックして、サイトのコンテンツをプレビュー用に公開します。
