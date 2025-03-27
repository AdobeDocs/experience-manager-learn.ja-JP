---
title: ユニバーサルエディターを使用した React アプリの編集
description: ユニバーサルエディターを使用してサンプル React アプリのコンテンツを編集する方法について説明します。
version: Experience Manager as a Cloud Service
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
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '415'
ht-degree: 100%

---

# ユニバーサルエディターを使用した React アプリの編集

ユニバーサルエディターを使用してサンプル React アプリのコンテンツを編集する方法について説明します。コンテンツは、AEM のコンテンツフラグメント内に保存され、GraphQL API を使用して取得されます。

このチュートリアルでは、ローカル開発環境の設定、React アプリの実装とそのコンテンツの編集、ユニバーサルエディターを使用したコンテンツの編集のプロセスについて順を追って説明します。

## 学習内容

このチュートリアルでは、以下のトピックを扱います。

- ユニバーサルエディターの簡単な概要
- ローカル開発環境の設定
   - **AEM SDK**：GraphQL API を使用して、React アプリのコンテンツフラグメント内に保存されたコンテンツを提供します。
   - **React アプリ**：AEM のコンテンツを表示するシンプルなユーザーインターフェイス。
   - **ユニバーサルエディターサービス**：ユニバーサルエディターと AEM SDK をバインドする&#x200B;_ユニバーサルエディターサービスのローカルコピー_。
- ユニバーサルエディターを使用してコンテンツを編集するために React アプリを実装する方法
- ユニバーサルエディターを使用して React アプリのコンテンツを編集する方法


## ユニバーサルエディターの概要

ユニバーサルエディターは、コンテンツ作成者と開発者（フロントエンドとバックエンド）をサポートします。ユニバーサルエディターの主なメリットのいくつかを確認してみましょう。

- ヘッドレスコンテンツとヘッドフルコンテンツを WYSIWYG（What You See Is What You Get）モードで編集できるように作成されています。
- React、Angular、Vue などの様々なフロントエンドテクノロジーをまたいで一貫したコンテンツ編集エクスペリエンスを提供します。したがって、コンテンツ作成者は、基になるフロントエンドテクノロジーを気にせずにコンテンツを編集できます。
- フロントエンドアプリケーションでユニバーサルエディターを有効にするには、最小限の実装が必要です。したがって、開発者の生産性が最大化され、エクスペリエンスの作成に焦点を合わせることができます。
- 懸念事項をコンテンツ作成者、フロントエンド開発者、バックエンド開発者の 3 つの役割に分けることで、各役割は自分たちの中核的な職務に集中できます。


## サンプル React アプリ

このチュートリアルでは、サンプル React アプリとして [**WKND Teams**](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/basic-tutorial#react-app---basic-tutorial---teampersons) を使用します。**WKND Teams** React アプリには、チームメンバーとその詳細のリストが表示されます。

タイトル、説明、チームメンバーなどのチームの詳細は、AEM に「_チーム_」コンテンツフラグメントとして保存されます。同様に、名前、略歴、プロファイル画像などのユーザーの詳細は、AEM に「_人物_」コンテンツフラグメントとして保存されます。

React アプリのコンテンツは、GraphQL API を使用して AEM によって提供され、ユーザーインターフェイスは、`Teams` と `Person` という 2 つの React コンポーネントを使用して作成されます。

**WKND Teams** React アプリの作成方法を学ぶには、対応するチュートリアルを参照してください。チュートリアルについては、[こちら](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/multi-step/overview)を参照してください。

## 次の手順

詳しくは、[ローカル開発環境の設定](./local-development-setup.md)方法を参照してください。
