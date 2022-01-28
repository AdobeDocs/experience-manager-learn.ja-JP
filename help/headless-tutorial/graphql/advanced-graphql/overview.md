---
title: AEMヘッドレスの高度な概念 — GraphQL
description: Adobe Experience Manager(AEM)GraphQL API の高度な概念を示す、エンドツーエンドのチュートリアルです。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Intermediate
source-git-commit: 83e16ea87847182139982ea2378d8ff9f079c968
workflow-type: tm+mt
source-wordcount: '926'
ht-degree: 1%

---

# AEMヘッドレスの高度な概念

このエンドツーエンドのチュートリアルは、 [基本チュートリアル](../multi-step/overview.md) Adobe Experience Manager(AEM) ヘッドレスと GraphQL の基本をカバーしています。 高度なチュートリアルでは、クライアントアプリケーションでのAEM GraphQL の使用を含め、コンテンツフラグメントモデル、コンテンツフラグメント、AEM GraphQL API の操作に関する詳細な側面を説明します。

## 前提条件

次を完了： [AEM as a Cloud Serviceのクイックセットアップ](../quick-setup/cloud-service.md) 環境を設定する場合。

前の [基本チュートリアル](../multi-step/overview.md) および [ビデオシリーズ](../video-series/modeling-basics.md) この高度なチュートリアルを進める前のチュートリアル ローカルのAEM環境を使用してチュートリアルを完了できますが、このチュートリアルでは、AEM as a Cloud Serviceのワークフローについてのみ説明します。

## 目的

このチュートリアルでは、次のトピックについて説明します。

* 検証ルールや、タブプレースホルダー、ネストされたフラグメント参照、JSON オブジェクト、日付と時刻データ型などの高度なデータ型を使用して、コンテンツフラグメントモデルを作成します。
* ネストされたコンテンツおよびフラグメント参照の操作中にコンテンツフラグメントを作成し、コンテンツフラグメントオーサリングガバナンス用のフォルダーポリシーを設定します。
* 変数とディレクティブを含む GraphQL クエリを使用してAEM GraphQL API 機能を調べます。
* AEMのパラメーターを使用して GraphQL クエリを保持し、永続クエリで cache-control パラメーターを使用する方法を学びます。
* AEMヘッドレス JavaScript SDK を使用して、永続化されたクエリのリクエストをサンプル WKND GraphQL React アプリに統合します。

## AEMヘッドレスの高度な概念の概要

次のビデオでは、このチュートリアルで扱う概念の概要を説明します。 このチュートリアルでは、より高度なデータ型を使用したコンテンツフラグメントモデルの定義、コンテンツフラグメントのネスト、AEMでの GraphQL クエリの保持について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/340035/?quality=12&learn=on)

## プロジェクト設定

WKND サイトプロジェクトには必要な設定がすべて含まれているので、チュートリアルは [クイック設定](../quick-setup/cloud-service.md). この節では、独自のAEMヘッドレスプロジェクトを作成する際に使用できる重要な手順についてのみ説明します。

### 設定の作成

AEMで新しいプロジェクトを開始する最初の手順は、ワークスペースとして設定を作成し、GraphQL API エンドポイントを作成することです。 設定を表示または作成するには、次の場所に移動します。 **ツール** > **一般** > **設定ブラウザー**.

![設定ブラウザーに移動します。](assets/overview/create-configuration.png)

WKND サイトの設定がこのチュートリアル用に既に作成されていることを確認します。 独自のプロジェクトの設定を作成するには、「 」を選択します。 **作成** 右上隅に表示される「設定を作成」モーダルのフォームに入力します。

### GraphQL API エンドポイントの作成

次に、GraphQL クエリを送信する API エンドポイントを設定する必要があります。 既存のエンドポイントを確認するか、作成するには、次の場所に移動します。 **ツール** > **Assets** > **GraphQL**.

![エンドポイントの設定](assets/overview/endpoints.png)

グローバルエンドポイントと WKND エンドポイントが既に作成されていることを確認します。 プロジェクトのエンドポイントを作成するには、「 **作成** 右上隅に移動し、ワークフローに従います。

>[!NOTE]
>
> エンドポイントを保存すると、セキュリティコンソールへのアクセスに関するモーダルが表示されます。このモーダルを使用すると、エンドポイントへのアクセスを設定する場合にセキュリティ設定を調整できます。 ただし、セキュリティ権限自体は、このチュートリアルの範囲外です。 詳しくは、 [AEMドキュメント](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=ja).

### プロジェクトの言語ルートフォルダーを作成する

言語ルートフォルダーは、EN や FR などの ISO 言語コードを名前として持つフォルダーです。 AEM翻訳管理システムは、これらのフォルダーを使用して、コンテンツの主要言語とコンテンツ翻訳の言語を定義します。

に移動します。 **ナビゲーション** > **Assets** > **ファイル**.

![ファイルに移動](assets/overview/files.png)

次に移動： **WKND サイト** フォルダー。 フォルダーのタイトルが「English」で、名前が「EN」であることを確認します。 このフォルダーは、WKND サイトプロジェクトの言語ルートフォルダーです。

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
* フォルダー階層は、常に平らで簡潔なものにする必要があります。 フォルダーとフラグメントの移動や名前の変更は、後で（特に実稼動用に公開した後に）おこなわないでください。公開すると、フラグメント参照や GraphQL クエリに影響を与える可能性のあるパスが変更されるからです。

## スターターおよびソリューションパッケージ

2 つのAEM **パッケージ** が使用可能で、 [パッケージマネージャー](/help/headless-tutorial/graphql/advanced-graphql/author-content-fragments.md#sample-content)

* [Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Starter-Package-1.0.zip) は、チュートリアルの後半で使用し、サンプル画像とフォルダを含みます。
* [Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/Advanced-GraphQL-Tutorial-Solution-Package-1.1.zip) には、新しいコンテンツフラグメントモデル、コンテンツフラグメント、永続化された GraphQL クエリを含む、第 1～4 章の完成したソリューションが含まれています。 右にスキップして [クライアントアプリケーションの統合](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md) チャプター。

2 つの React JS プロジェクトを使用してクエリを試すことができます。 [ヘッドレスクライアントアプリケーションから](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).

* [aem-guides-wknd-headless-start-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-start-tutorial.zip)  — で完了したスタータークライアントアプリケーション [第 5 章 — クライアントアプリケーションの統合](/help/headless-tutorial/graphql/advanced-graphql/client-application-integration.md).
* [aem-guides-wknd-headless-solution-tutorial.zip](/help/headless-tutorial/graphql/advanced-graphql/assets/tutorial-files/aem-guides-wknd-headless-solution-tutorial.zip)  — を使用して終了したクライアントアプリケーション **持続** クエリ。


## 概要

この高度なチュートリアルを開始するには、次の手順に従います。

1. を使用した開発環境の設定 [AEMas a Cloud Service](../quick-setup/cloud-service.md).
1. 次のチュートリアルの章を開始します。 [コンテンツフラグメントモデルの作成](/help/headless-tutorial/graphql/advanced-graphql/create-content-fragment-models.md).
