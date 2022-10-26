---
title: AEM SPA Editor と React の使用の手引き
description: WKND SPA を使用して、Adobe Experience Manager（AEM）で編集可能な最初の React 単一ページアプリケーション（SPA）を作成します。AEM の SPA Editor で React JS フレームワークを使用して SPA を作成する方法について学習します。このマルチパートチュートリアルでは、架空のライフスタイルブランドである WKND 向けの React アプリケーションの実装について説明します。このチュートリアルでは、SPA のエンドツーエンドの作成と AEM との統合について説明します。
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: 38802296-8988-4300-a04a-fcbbe98ac810
last-substantial-update: 2022-08-25T00:00:00Z
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 35%

---

# AEM での React SPA の作成（チュートリアル） {#overview}

このたびは、 **SPA Editor** Adobe Experience Manager(AEM) の機能を使用できます。 このチュートリアルでは、架空のライフスタイルブランドである WKND 向けの React アプリケーションの実装について説明します。 React アプリは、AEM SPA Editor を使用してデプロイされるように開発および設計されています。このエディターは、React コンポーネントをAEMコンポーネントにマッピングします。 AEMにデプロイされた完成したSPAは、AEMの従来のインライン編集ツールを使用して動的にオーサリングできます。

![最終的なSPAの実装](assets/wknd-spa-implementation.png)

*WKND SPAの実装*

## について

このチュートリアルは、 **AEMas a Cloud Service** との後方互換性がある **AEM 6.5.4 以降** および **AEM 6.4.8 以降**.

## 最新のコード

チュートリアルコードはすべて、 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

この [最新のコードベース](https://github.com/adobe/aem-guides-wknd-spa/releases) は、ダウンロード可能なAEMパッケージとして入手できます。

## 前提条件

このチュートリアルを開始する前に、次の手順を実行する必要があります。

* HTML、CSS、JavaScript に関する基本的な知識
* ～に対する基本的な知識 [React](https://reactjs.org/tutorial/tutorial.html)

*必須ではありませんが、基本的な理解を持つことは有益です [従来のAEM Sitesコンポーネントの開発](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ja).*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、Mac OS 環境で動作しているAEMas a Cloud ServiceSDK を使用して、 [Visual Studio Code](https://code.visualstudio.com/) を IDE として使用します。 特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムから独立している必要があります。

### 必要なソフトウェア

* [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja), [AEM 6.5.4 以降](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65) または [AEM 6.4.8 以降](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.js](https://nodejs.org/ja/) および [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **AEM as a Cloud Service は初めてですか？** 以下を確認します。 [AEM as a Cloud Service SDK を使用したローカル開発環境の設定に関する以下のガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja).
>
> **AEM 6.5 を初めて使用する場合** 以下を確認します。 [次のガイドでは、ローカル開発環境を設定します](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja).

## 次の手順 {#next-steps}

何を待ってる?!チュートリアルを開始するには、 [プロジェクトを作成](create-project.md) この章では、AEMプロジェクトアーキタイプを使用してSPA Editor が有効なプロジェクトを生成する方法について説明します。
