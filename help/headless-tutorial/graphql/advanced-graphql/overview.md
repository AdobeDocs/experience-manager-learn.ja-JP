---
title: AEMヘッドレスの高度な概念 — GraphQL
description: Adobe Experience Manager(AEM)GraphQL API の高度な概念を示す、エンドツーエンドのチュートリアルです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
exl-id: daae6145-5267-4958-9abe-f6b7f469f803
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '1076'
ht-degree: 1%

---

# AEMヘッドレスの高度な概念

このエンドツーエンドのチュートリアルは、 [基本チュートリアル](../multi-step/overview.md) Adobe Experience Manager(AEM) ヘッドレスとGraphQLの基本をカバーしています。 高度なチュートリアルでは、GraphQLでの永続化クエリの使用など、コンテンツフラグメントモデル、コンテンツフラグメント、AEM GraphQLでのクエリの操作に関する詳細な側面を説明します。

## 前提条件

次を完了： [AEM as a Cloud Serviceのクイックセットアップ](../quick-setup/cloud-service.md) AEM as a Cloud Service環境を設定する場合。

前の [基本チュートリアル](../multi-step/overview.md) および [ビデオシリーズ](../video-series/modeling-basics.md) この高度なチュートリアルを進める前のチュートリアル ローカルのAEM環境を使用してチュートリアルを完了できますが、このチュートリアルでは、AEM as a Cloud Serviceのワークフローについてのみ説明します。

>[!CAUTION]
>
>AEMas a Cloud Service環境にアクセスできない場合は、 [ローカル SDK を使用したAEMヘッドレスクイックセットアップ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/quick-setup/local-sdk.html). ただし、コンテンツフラグメントナビゲーションなど、一部の製品 UI ページは異なることに注意する必要があります。



## 目的

このチュートリアルでは、次のトピックについて説明します。

* 検証ルールや、タブプレースホルダー、ネストされたフラグメント参照、JSON オブジェクト、日付と時刻データ型などの高度なデータ型を使用して、コンテンツフラグメントモデルを作成します。
* ネストされたコンテンツおよびフラグメント参照の操作中にコンテンツフラグメントを作成し、コンテンツフラグメントオーサリングガバナンス用のフォルダーポリシーを設定します。
* 変数およびディレクティブを含むGraphQLクエリを使用してAEM GraphQL API 機能を調べます。
* AEMのパラメーターを使用してGraphQLクエリを保持し、永続化されたクエリで cache-control パラメーターを使用する方法を学びます。
* AEMヘッドレス JavaScript SDK を使用して、永続化されたクエリのリクエストをサンプル WKND GraphQL React アプリに統合します。

## AEMヘッドレスの高度な概念の概要

次のビデオでは、このチュートリアルで扱う概念の概要を説明します。 このチュートリアルでは、より高度なデータ型を使用したコンテンツフラグメントモデルの定義、コンテンツフラグメントのネスト、AEMでのGraphQLクエリの保持について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/340035?quality=12&learn=on)

>[!CAUTION]
>
>このビデオ (2:25) では、パッケージマネージャーを使用して GraphiQL クエリエディターをインストールし、GraphQLクエリを調査する方法について説明します。 ただし、新しいバージョンのAEM as Cloud Serviceには組み込み **GraphiQL エクスプローラ** が指定されているので、パッケージのインストールは不要です。 詳しくは、 [GraphiQL IDE の使用](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/graphiql-ide.html) を参照してください。


## プロジェクト設定

WKND サイトプロジェクトには必要な設定がすべて含まれているので、チュートリアルは [クイック設定](../quick-setup/cloud-service.md). この節では、独自のAEMヘッドレスプロジェクトを作成する際に使用できる重要な手順についてのみ説明します。


### 既存の設定を確認

AEMで新しいプロジェクトを開始する最初の手順は、ワークスペースとしての設定を作成し、GraphQL API エンドポイントを作成することです。 設定を確認または作成するには、次の場所に移動します。 **ツール** > **一般** > **設定ブラウザー**.

![設定ブラウザーに移動します。](assets/overview/create-configuration.png)

次の点に注意してください。 `WKND Shared` このチュートリアルでは、サイト設定が既に作成されています。 独自のプロジェクトの設定を作成するには、「 」を選択します。 **作成** 右上隅に表示される「設定を作成」モーダルのフォームに入力します。

![WKND 共有設定の確認](assets/overview/review-wknd-shared-configuration.png)

### GraphQL API エンドポイントの確認

次に、GraphQLクエリを送信する API エンドポイントを設定する必要があります。 既存のエンドポイントを確認するか、作成するには、次の場所に移動します。 **ツール** > **一般** > **GraphQL**.

![エンドポイントの設定](assets/overview/endpoints.png)

次の点に注意してください。 `WKND Shared Endpoint` は既に作成されています。 プロジェクトのエンドポイントを作成するには、「 **作成** 右上隅に移動し、ワークフローに従います。

![WKND 共有エンドポイントを確認](assets/overview/review-wknd-shared-endpoint.png)

>[!NOTE]
>
> エンドポイントを保存すると、セキュリティコンソールへのアクセスに関するモーダルが表示されます。このモーダルを使用すると、エンドポイントへのアクセスを設定する場合にセキュリティ設定を調整できます。 ただし、セキュリティ権限自体は、このチュートリアルの範囲外です。 詳しくは、 [AEMドキュメント](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=ja).

### WKND コンテンツ構造と言語ルートフォルダーの確認

明確に定義されたコンテンツ構造は、AEMヘッドレス実装を成功させるうえで重要です。 コンテンツの拡張性、操作性、権限管理に役立ちます。

言語ルートフォルダーは、EN や FR などの ISO 言語コードを名前として持つフォルダーです。 AEM翻訳管理システムは、これらのフォルダーを使用して、コンテンツの主要言語とコンテンツ翻訳の言語を定義します。

に移動します。 **ナビゲーション** > **Assets** > **ファイル**.

![ファイルに移動](assets/overview/files.png)

次に移動： **WKND 共有** フォルダー。 フォルダーのタイトルが「English」で、名前が「EN」であることを確認します。 このフォルダーは、WKND サイトプロジェクトの言語ルートフォルダーです。

![英語フォルダー](assets/overview/english.png)

独自のプロジェクトの場合、設定内に言語ルートフォルダーを作成します。 詳しくは、 [フォルダの作成](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#create-folders) を参照してください。

### ネストされたフォルダーに設定を割り当てる

最後に、プロジェクトの設定を言語ルートフォルダーに割り当てる必要があります。 この割り当てにより、プロジェクトの設定で定義されたコンテンツフラグメントモデルに基づいて、コンテンツフラグメントを作成できます。

言語ルートフォルダーを設定に割り当てるには、フォルダーを選択してから、「 」を選択します。 **プロパティ** をクリックします。

![プロパティを選択](assets/overview/properties.png)

次に、 **Cloud Services** 」タブをクリックし、 **クラウド設定** フィールドに入力します。

![クラウド設定](assets/overview/cloud-conf.png)

表示されるモーダルで、以前に作成した設定を選択して、その設定に言語ルートフォルダーを割り当てます。

### ベストプラクティス

AEMで独自のプロジェクトを作成する際のベストプラクティスは次のとおりです。

* フォルダー階層は、ローカライゼーションと翻訳を念頭に置いてモデル化する必要があります。 つまり、言語フォルダーは設定フォルダー内にネストする必要があります。これにより、設定フォルダー内のコンテンツを簡単に翻訳できます。
* フォルダー階層は、常に平らで簡潔なものにする必要があります。 フォルダーとフラグメントの移動や名前の変更は後でおこなわないでください。特に、実稼働環境での公開後に、フラグメント参照やGraphQLクエリに影響を及ぼす可能性のあるパスが変更されるので、名前を変更しません。

## スターターおよびソリューションパッケージ

2 つのAEM **パッケージ** が使用可能で、 [パッケージマネージャー](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.1.zip) は、チュートリアルの後半で使用し、サンプル画像とフォルダを含みます。
* [Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.2.zip) には、新しいコンテンツフラグメントモデル、コンテンツフラグメント、永続化されたGraphQLクエリを含む、第 1 ～ 4 章の完成したソリューションが含まれています。 右にスキップして [クライアントアプリケーションの統合](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) チャプター。


この [React アプリ — 高度なチュートリアル — WKND アドベンチャ](https://github.com/adobe/aem-guides-wknd-graphql/blob/main/advanced-tutorial/README.md) プロジェクトを使用して、サンプルアプリケーションを確認および調査できます。 このサンプルアプリケーションは、永続化されたGraphQLクエリを呼び出してAEMからコンテンツを取得し、没入感のあるエクスペリエンスでレンダリングします。

## 概要

この高度なチュートリアルを開始するには、次の手順に従います。

1. を使用した開発環境の設定 [AEMas a Cloud Service](../quick-setup/cloud-service.md).
1. 次のチュートリアルの章を開始します。 [コンテンツフラグメントモデルの作成](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
