---
title: AEM SPA Editor と React の使用の手引き
description: WKND SPAを使用して、Adobe Experience Manager AEMで編集可能な最初のReactシングルページアプリケーション(SPA)を作成します。 AEM SPA EditorでReact JSフレームワークを使用してSPAを作成する方法を説明します。 このマルチパートチュートリアルでは、架空のライフスタイルブランドであるWKND向けのReactアプリケーションの実装について説明します。 このチュートリアルでは、SPAのエンドツーエンドの作成とAEMとの統合について説明します。
sub-product: sites
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
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 16%

---

# AEM での React SPA の作成（チュートリアル） {#overview}

このたびは、Adobe Experience Manager(AEM)の&#x200B;**SPA Editor**&#x200B;機能を初めて使用する開発者向けに設計された、マルチパートチュートリアルをご利用いただき、誠にありがとうございます。 このチュートリアルでは、架空のライフスタイルブランドであるWKND向けのReactアプリケーションの実装について説明します。 Reactアプリは、AEM SPA Editorを使用してデプロイされるように開発および設計されます。このエディターは、ReactコンポーネントをAEMコンポーネントにマッピングします。 AEMにデプロイされた完成したSPAは、AEMの従来のインライン編集ツールを使用して動的にオーサリングできます。

![最終的なSPAの実装](assets/wknd-spa-implementation.png)

*WKND SPAの実装*

## について

このチュートリアルは、**AEMをCloud Service**&#x200B;として使用するように設計されており、**AEM 6.5.4+**&#x200B;および&#x200B;**AEM 6.4.8+**&#x200B;と後方互換性があります。

## 最新コード

チュートリアルのコードはすべて[GitHub](https://github.com/adobe/aem-guides-wknd-spa)にあります。

[最新のコードベース](https://github.com/adobe/aem-guides-wknd-spa/releases)は、ダウンロード可能なAEMパッケージとして入手できます。

## 前提条件

このチュートリアルを開始する前に、次が必要です。

* HTML、CSS、JavaScriptに関する基本的な知識
* [React](https://reactjs.org/tutorial/tutorial.html)に対する基本的な知識

*必須ではありませんが、従来のAEM Sitesコンポーネントの開発に関する基本的な [知識を持つことは有益です](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、Mac OS環境で動作するAEM as aCloud ServiceSDK（[Visual Studio Code](https://code.visualstudio.com/)をIDEとして使用）を使用してキャプチャされます。 特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムとは独立している必要があります。

### 必要なソフトウェア

* [AEM as aCloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html)、 [AEM 6.5.4以降、](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-65)  [AEM 6.4.8以降](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/aem-releases-updates.html?lang=en#aem-64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.](https://nodejs.org/ja/) jsand  [npm](https://www.npmjs.com/)

>[!NOTE]
>
> **AEM as a Cloud Service は初めてですか？** AEM as a  [Cloud ServiceSDKを使用したローカル開発環境のセットアップについては、次のガイドを参照してください](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5を初めて使用する場合** 次のガイドを参照し [て、ローカル開発環境を設定します](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja)。

## 次の手順 {#next-steps}

何を待ってる?!チュートリアルを開始するには、「[プロジェクトの作成](create-project.md)」の章に移動し、AEMプロジェクトアーキタイプを使用してSPA Editor対応プロジェクトを生成する方法を学びます。
