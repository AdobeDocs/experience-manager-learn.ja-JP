---
title: ユニバーサルエディターを使用した React アプリの編集
description: ユニバーサルエディターを使用してサンプル React アプリのコンテンツを編集する方法を説明します。
version: Cloud Service
feature: Developer Tools, Headless
topic: Development, Content Management
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 87
last-substantial-update: 2024-04-19T00:00:00Z
jira: KT-15359
thumbnail: KT-15359.png
exl-id: 69ed610e-2eff-43b3-98f9-3dc40594e879
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 1%

---

# ユニバーサルエディターを使用した React アプリの編集

ユニバーサルエディターを使用してサンプル React アプリのコンテンツを編集する方法を説明します。 コンテンツはAEMのコンテンツフラグメントに保存され、GraphQL API を使用して取得されます。

このチュートリアルでは、ローカル開発環境のセットアップ、React アプリの実装とそのコンテンツの編集、ユニバーサルエディターを使用したコンテンツの編集のプロセスについて説明します。

## 学習内容

このチュートリアルでは、以下のトピックを扱います。

- ユニバーサルエディターの概要
- ローカル開発環境のセットアップ
   - **AEM SDK**:GraphQL API を使用して、React アプリのコンテンツフラグメント内に保存されたコンテンツを提供します。
   - **React アプリ**:AEMのコンテンツを表示するシンプルなユーザーインターフェイス。
   - **ユニバーサルエディターサービス**：ユニバーサルエディターとAEM SDK をバインドする _ユニバーサルエディターサービスのローカルコピー_。
- ユニバーサルエディターを使用してコンテンツを編集するための React アプリのインストルメント方法
- ユニバーサルエディターを使用して React アプリのコンテンツを編集する方法


## ユニバーサルエディターの概要

ユニバーサルエディターはコンテンツ作成者と開発者（フロントエンドおよびバックエンド）を強化します。次に、ユニバーサルエディターの主なメリットをいくつか見てみましょう。

- ヘッドレスコンテンツやヘッドフルコンテンツを WYSIWYG （what-you-see-is-what-you-get）モードで編集するために構築されています。
- React、Angular、Vue など、様々なフロントエンドテクノロジーをまたいで一貫したコンテンツ編集エクスペリエンスを提供します。 これにより、コンテンツ作成者は、基盤となるフロントエンドテクノロジーを気にすることなくコンテンツを編集できます。
- フロントエンドアプリケーションでユニバーサルエディターを有効にするには、最小限の実装が必要です。 したがって、開発者の生産性を最大化し、開発者を解放してエクスペリエンスの構築に集中させることができます。
- コンテンツ作成者、フロントエンド開発者、バックエンド開発者の 3 つの役割にわたる懸念の分離。各役割が中核的な責任に集中できるようになります。


## サンプル React アプリ

このチュートリアルでは [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) をサンプル React アプリとして使用します。 **WKND チーム** の React アプリは、チームメンバーとその詳細のリストを表示します。

チームの詳細（タイトル、説明、チームメンバーなど）は、AEMの _チーム_ コンテンツフラグメントとして保存されます。 同様に、人物の詳細（名前、伝記、プロファイル画像など）は、AEMで _人物_ コンテンツフラグメントとして保存されます。

React アプリのコンテンツは、GraphQL API を使用してAEMによって提供され、ユーザーインターフェイスは、`Teams` と `Person` の 2 つの React コンポーネントを使用して作成されます。

**WKND Teams** React アプリの作成方法については、対応するチュートリアルを参照してください。 チュートリアルは [ こちら ](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview) にあります。

## 次の手順

ローカル開発環境の設定 [ 方法について説明し ](./local-development-setup.md) す。
