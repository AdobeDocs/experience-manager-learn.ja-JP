---
title: SPA EditorとRemote SPAの概要 — 概要
description: AEM SPAエディタを使用して、既存のリモートSPAに編集可能なAEMコンテンツを追加する開発者向けのマルチパートチュートリアルをご利用いただけます。
topic: ヘッドレス、SPA、開発
feature: SPAエディター，コアコンポーネント， API，開発
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: kt-7630.jpg
translation-type: tm+mt
source-git-commit: 0eb086242ecaafa53c59c2018f178e15f98dd76f
workflow-type: tm+mt
source-wordcount: '675'
ht-degree: 5%

---


# 概要

SPA editorを使用して、既存のReact-based（またはNext.js）リモートSPAと編集可能なAEMコンテンツを拡張する開発者向けのマルチパートチュートリアルをご利用いただけます。

このチュートリアルは、[WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ja)上に構築されています。AEM GraphQL APIではAEMコンテンツフラグメントコンテンツを使用するReactアプリですが、SPAコンテンツの文脈依存オーサリングは提供されません。

## このチュートリアルについて

このチュートリアルでは、リモートSPA、またはAEMのコンテキスト外で実行されるSPAを、AEMで作成されたコンテンツを使用して配信するように更新する方法を説明します。

チュートリアルのほとんどのアクティビティはJavaScriptの開発に重点を置いていますが、AEMに関して重要な側面が取り上げられています。 これらの側面には、AEMでコンテンツが作成され、保存される場所の定義や、SPAのAEMページへのルートのマッピングなどが含まれます。

このチュートリアルは、**AEMをCloud Service**&#x200B;として使用するように設計されており、2つのプロジェクトで構成されています。

1. __AEMプロジェクト__&#x200B;には、AEMに展開する必要がある設定とコンテンツが含まれています。
1. __WKND__ Appprojectは、AEM editorに統合されるSPAです

## 最新のコード

+ このチュートリアルのコードは、`feature/spa-editor`ブランチの[GitHub](https://github.com/adobe/aem-guides-wknd-graphq)にあります。

## 前提条件

このチュートリアルでは、以下が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=en)
+ [Node.js v14 以降](https://nodejs.org/ja/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphqlソースコード](https://github.com/adobe/aem-guides-wknd-graphql)

このチュートリアルでは、次の点を前提としています。

+ [IDEのMicrosoft® Visual Studio](https://visualstudio.microsoft.com/) コード
+ `~/Code/wknd-app`の作業ディレクトリ
+ `http://localhost:4502`上での作成者サービスとしてのAEM SDKの実行
+ ローカルの`admin`アカウントとパスワード`admin`を使用したAEM SDKの実行
+ `http://localhost:3000`上でSPAを実行する

>[!NOTE]
>
> **ローカル開発環境の設定に関するヘルプが必要ですか？** AEMをCloud ServiceSDKとして使用するローカル開発環境を設定するには、 [次のガイドを参照してください](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。


## クイックセットアップ

WKND App SPAおよびAEM SPA Editorを15分で起動および実行できます。 この高速設定では、チュートリアルの最後の状態に直接進み、AEM SPAエディタでSPAのオーサリングを試すことができます。

+ [クイックセットアップ](./quick-setup.md)

## SPAエディタ用のAEMの設定

SPAをSPA editorと統合するには、AEMの設定が必要です。 これらの設定は、AEMプロジェクトで管理および展開されます。 この章では、設定が必要となるものと、設定を定義する方法について説明します。

+ [AEM の設定](./aem-configure.md)

## BootstrapSPA

AEM SPAエディタでSPAをオーサリングコンテキストに統合するには、SPAにいくつかの追加が必要です。

+ [SPA for SPA EditorのBootstrap](./spa-bootstrap.md)

## 編集可能な固定コンポーネント

まず、編集可能な「固定コンポーネント」をSPAに追加します。 この例では、開発者がSPA内に特定の編集可能コンポーネントを配置する方法を示します。 作成者はコンポーネントのコンテンツを変更できますが、コンポーネントを削除したり、配置、位置、サイズを変更したりすることはできません。

+ [編集可能な固定コンポーネント](./spa-fixed-component.md)

## 編集可能なコンテナコンポーネント

次に、編集可能な「コンテナコンポーネント」をSPAに追加します。 これは、開発者がSPAにコンテナコンポーネントを配置する方法を示します。 コンテナコンポーネントを使用すると、作成者は許可されたコンポーネントをその中に配置し、コンポーネントのレイアウトを調整できます。

## 動的ルートと編集可能コンポーネント

最後に、前章で説明した概念を使用して、動的ルートを設定します。ルートのパラメーターに基づいて異なるコンテンツを表示するルート。 次に、プログラムによって駆動され、派生するルートのコンテンツをAEM SPA Editorを使用して作成する方法を示します。

+ [ダイナミックルートと編集可能コンポーネント](./spa-dynamic-routes.md)

## その他のリソース

+ [AEMドキュメント内の外部SPAの編集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCMコンポーネント — リアクトコアの実装](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCMコンポーネント — Spaエディタ — React Core実装](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
