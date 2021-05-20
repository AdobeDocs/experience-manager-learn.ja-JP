---
title: SPA EditorおよびRemote SPAの概要
description: AEM SPA Editorを使用して、既存のリモートSPAに編集可能なAEMコンテンツを拡張する開発者向けの、複数のパートから成るチュートリアルへようこそ。
topic: ヘッドレス、SPA、開発
feature: SPAエディター、コアコンポーネント、API、開発
role: Developer, Architect
level: Beginner
kt: 7630
thumbnail: 333272.jpeg
source-git-commit: cede0c97e0f322fe5d20d5c4f685ed10b90af1d4
workflow-type: tm+mt
source-wordcount: '694'
ht-degree: 6%

---


# 概要

AEM SPA Editorを使用して、既存のReactベース（またはNext.js）のリモートSPAに編集可能なAEMコンテンツを拡張する開発者向けの、マルチパートチュートリアルへようこそ。

このチュートリアルは、 AEM GraphQL APIを介してAEMコンテンツフラグメントコンテンツを使用するReactアプリである[WKND GraphQL App](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ja)に基づいて構築されますが、SPAコンテンツのコンテキスト内オーサリングは提供されません。

>[!VIDEO](https://video.tv.adobe.com/v/333272/?quality=12&learn=on)

## このチュートリアルについて

AEMのコンテキスト外で動作するリモートSPAまたはSPAを更新して、AEMで作成したコンテンツを使用および配信する方法を説明するためのチュートリアルです。

チュートリアルのほとんどのアクティビティはJavaScriptの開発に焦点を当てていますが、AEMを中心とした重要な側面について説明しています。 これらの側面には、コンテンツのオーサリングおよび格納先の定義や、SPAルートのAEMページへのマッピングが含まれます。

このチュートリアルは、**AEMをCloud Service**&#x200B;として使用するように設計されており、次の2つのプロジェクトで構成されています。

1. __AEMプロジェクト__&#x200B;には、AEMにデプロイする必要がある設定とコンテンツが含まれます。
1. __WKND__ Appprojectは、AEM SPA Editorと統合されるSPAです

## 最新のコード

+ このチュートリアルのコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-graphql)の`feature/spa-editor`ブランチにあります。

## 前提条件

このチュートリアルでは、次の項目が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja)
+ [Node.js v14 以降](https://nodejs.org/ja/)
+ [npm v7+](https://www.npmjs.com/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6以降](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all.0.3.0.zip以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphqlソースコード](https://github.com/adobe/aem-guides-wknd-graphql)

このチュートリアルでは、次の点を前提としています。

+ [IDEとしてのMicrosoft® Visual Studio ](https://visualstudio.microsoft.com/) Codeas
+ `~/Code/wknd-app`の作業ディレクトリ
+ `http://localhost:4502`でのAEM SDKのオーサーサービスとしての実行
+ パスワード`admin`を使用したローカル`admin`アカウントでのAEM SDKの実行
+ `http://localhost:3000`でのSPAの実行

>[!NOTE]
>
> **ローカル開発環境の設定に関するヘルプが必要な場合** AEM as a  [Cloud ServiceSDKを使用したローカル開発環境のセットアップについては、次のガイドを参照してください](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。


## クイックセットアップ

クイックセットアップを使用すると、WKND App SPA and AEM SPA Editorを15分で使い始めることができます。 この高速化されたセットアップでは、チュートリアルの最後の段階に直接進み、AEM SPA EditorでのSPAのオーサリングを検討できます。

+ [クイックセットアップの詳細](./quick-setup.md)

## 1. AEMをSPA Editor用に設定

SPAをAEM SPA Editorと統合するには、AEMの設定が必要です。 これらの設定は、AEMプロジェクトを介して管理およびデプロイされます。 この章では、必要な設定とその定義方法について説明します。

+ [AEM for SPA Editorの設定方法を説明します。](./aem-configure.md)

## 2. SPAのBootstrap

AEM SPA EditorをSPAのオーサリングコンテキストに統合するには、SPAに対していくつかの追加を行う必要があります。

+ [SPA for AEM SPA Editorのブートストラップ方法を説明します。](./spa-bootstrap.md)

## 3.編集可能な固定コンポーネント

まず、編集可能な「固定コンポーネント」をSPAに追加します。 この例では、開発者が特定の編集可能なコンポーネントをSPAに配置する方法を示します。 作成者はコンポーネントのコンテンツを変更できますが、コンポーネントを削除したり、その配置、配置またはサイズを変更したりすることはできません。

+ [編集可能な固定コンポーネントについて説明します](./spa-fixed-component.md)

## 4.編集可能なコンテナコンポーネント

次に、編集可能な「コンテナコンポーネント」をSPAに追加する方法を説明します。 この例では、開発者がSPAにコンテナコンポーネントを配置する方法を示します。 コンテナコンポーネントを使用すると、作成者は許可されたコンポーネントをその中に配置したり、コンポーネントのレイアウトを調整したりできます。

+ [編集可能なコンテナコンポーネントについて説明します](./spa-container-component.md)

## 5.動的ルートおよび編集可能なコンポーネント

最後に、前の章で説明した概念を使用して、動的ルートを設定します。ルートのパラメーターに基づいて異なるコンテンツを表示するルート。 この例では、AEM SPA Editorを使用して、プログラムによって駆動され、派生されるルートのコンテンツを作成する方法を示します。

+ [動的ルートと編集可能なコンポーネントについて説明します](./spa-dynamic-routes.md)

## その他のリソース

+ [AEMドキュメント内の外部SPAの編集](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/hybrid/editing-external-spa.html)
+ [AEM WCMコンポーネント — Reactコア実装](https://www.npmjs.com/package/@adobe/aem-core-components-react-base)
+ [AEM WCMコンポーネント — Spaエディター — React Core実装](https://www.npmjs.com/package/@adobe/aem-core-components-react-spa)
