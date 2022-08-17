---
title: SPA Editor と Remote SPAの概要 — 概要
description: AEM SPA Editor を使用して編集可能なAEMコンテンツを既存の Remote SPAに拡張する開発者向けの、複数のパートから成るチュートリアルへようこそ。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
source-git-commit: fe056006ab59a3955e5f16a23e96e9e208408cf5
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 8%

---

# 概要

このたびは、AEM SPA Editor を使用して、既存の React ベース（または Next.js）のリモートSPAに編集可能なAEMコンテンツを拡張する開発者向けの、マルチパートチュートリアルをご利用いただき、誠にありがとうございます。

このチュートリアルでは、 [WKND GraphQL アプリ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ja):AEM GraphQL API を介してAEMコンテンツフラグメントコンテンツを使用する React アプリですが、SPAコンテンツのコンテキスト内オーサリングは提供されていません。

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## このチュートリアルについて

AEMのコンテキスト外で実行されるSPAやリモートSPAを更新して、AEMで作成したコンテンツを使用および配信する方法を説明するためのチュートリアルです。

このチュートリアルのほとんどのアクティビティは JavaScript の開発に焦点を当てていますが、AEMを中心に重要な側面について説明しています。 これらの側面には、コンテンツのオーサリングおよび格納場所の定義や、SPAルートのAEM Pages へのマッピングが含まれます。

このチュートリアルは、 **AEMas a Cloud Service** とは、次の 2 つのプロジェクトで構成されています。

1. この __AEM Project__ には、AEMにデプロイする必要がある設定とコンテンツが含まれています。
1. __WKND アプリ__ プロジェクトは、AEM SPA Editor と統合するSPAです

## 最新のコード

+ このチュートリアルのコードは、 [GitHub](https://github.com/adobe/aem-guides-wknd-graphql) の `feature/spa-editor` 分岐。

## 前提条件

このチュートリアルでは、次の項目が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja)
+ [Node.js v16 以降](https://nodejs.org/ja/)
+ [npm v8+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6 以降](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip 以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql ソースコード ( ブランチ：feature/spa-editor)](https://github.com/adobe/aem-guides-wknd-graphql/tree/feature/spa-editor)

このチュートリアルでは、次を前提としています。

+ [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/) IDE として
+ の作業ディレクトリ `~/Code/wknd-app`
+ AEM SDK as a Author サービスを `http://localhost:4502`
+ ローカルでのAEM SDK の実行 `admin` パスワードを持つアカウント `admin`
+ でのSPAの実行 `http://localhost:3000`

>[!NOTE]
>
> **ローカル開発環境の設定に関するヘルプが必要ですか？** 以下を確認します。 [AEM as a Cloud Service SDK を使用したローカル開発環境の設定に関する以下のガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja).


## クイックセットアップ

クイックセットアップを使用すると、WKND App SPAおよびAEM SPA Editor を 15 分で使い始めることができます。 この高速設定により、チュートリアルの最後の状態に直接移動し、AEM SPA Editor でのSPAのオーサリングを確認できます。

+ [クイックセットアップの詳細](./quick-setup.md)

## 1. AEM for SPA Editor を設定する

SPAをAEM SPA Editor と統合するには、AEMの設定が必要です。 これらの設定は、AEMプロジェクトを介して管理およびデプロイされます。 この章では、必要な設定とその定義方法について説明します。

+ [AEM for SPA Editor の設定方法を説明します。](./aem-configure.md)

## 2. SPAのBootstrap

AEM SPA Editor をSPAのオーサリングコンテキストに統合するには、SPAに対していくつかの追加をおこなう必要があります。

+ [SPA for AEM SPA Editor のブートストラップ方法を説明します。](./spa-bootstrap.md)

## 3.編集可能な固定コンポーネント

まず、編集可能な「固定コンポーネント」をSPAに追加します。 この例では、開発者が特定の編集可能なコンポーネントをSPAに配置する方法を示します。 作成者はコンポーネントのコンテンツを変更できますが、コンポーネントを削除したり、コンポーネントの配置、配置、サイズを変更したりすることはできません。

+ [編集可能な固定コンポーネントについて説明します](./spa-fixed-component.md)

## 4.編集可能なコンテナコンポーネント

次に、編集可能な「コンテナコンポーネント」をSPAに追加する方法を説明します。 この例では、開発者がSPAにコンテナコンポーネントを配置する方法を示します。 コンテナコンポーネントを使用すると、作成者は許可されたコンポーネントをその中に配置したり、コンポーネントのレイアウトを調整したりできます。

+ [編集可能なコンテナコンポーネントについて説明します](./spa-container-component.md)

## 5.動的ルートと編集可能コンポーネント

最後に、前の章で説明した概念を使用して、動的ルートを設定します。ルートのパラメータに基づいて異なるコンテンツを表示するルート。 この例では、AEM SPA Editor を使用して、プログラムによって駆動され、派生されるルートのコンテンツを作成する方法を示します。

+ [動的ルートと編集可能なコンポーネントについて説明します](./spa-dynamic-routes.md)

## その他のリソース

+ [AEM Docs 内での外部SPAの編集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCM コンポーネント — React Core 実装](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCM コンポーネント — Spa エディター — React Core 実装](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
