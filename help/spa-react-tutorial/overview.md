---
title: AEM SPA Editor と React の使用の手引き
description: WKND SPAを使用して、Adobe Experience Manager AEMで編集可能な最初のReactシングルページアプリケーション(SPA)を作成します。 AEM SPA EditorでReact JSフレームワークを使用してSPAを作成する方法を説明します。 このマルチパートチュートリアルでは、架空のライフスタイルブランドであるWKND向けのReactアプリケーションの実装について説明します。 このチュートリアルでは、SPAのエンドツーエンドの作成とAEMとの統合について説明します。
sub-product: サイト
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '732'
ht-degree: 18%

---


# AEM での React SPA の作成（チュートリアル） {#overview}

このたびは、Adobe Experience Manager(AEM)の&#x200B;**SPA Editor**&#x200B;機能を初めて使用する開発者向けに設計された、マルチパートチュートリアルをご利用いただき、誠にありがとうございます。 このチュートリアルでは、架空のライフスタイルブランドであるWKND向けのReactアプリケーションの実装について説明します。 Reactアプリは、AEM SPA Editorを使用してデプロイされるように開発および設計されます。このエディターは、ReactコンポーネントをAEMコンポーネントにマッピングします。 AEMにデプロイされた完成したSPAは、AEMの従来のインライン編集ツールを使用して動的にオーサリングできます。

![最終的なSPAの実装](assets/wknd-spa-implementation.png)

*WKND SPAの実装*

##  について

このマルチパートチュートリアルの目標は、AEMのSPA Editor機能と連携するReactアプリケーションの実装方法を開発者に教えることです。 実際のシナリオでは、開発アクティビティはペルソナ別に分類され、多くの場合、**フロントエンド開発者**&#x200B;と&#x200B;**バックエンド開発者**&#x200B;が関与します。 このチュートリアルを完了するには、AEM SPA Editorプロジェクトに参加する開発者にとって有益です。

このチュートリアルは、**AEMをCloud Service**&#x200B;として使用するように設計されており、**AEM 6.5.4+**&#x200B;および&#x200B;**AEM 6.4.8+**&#x200B;と後方互換性があります。 SPAは、次を使用して実装されます。

* [Maven AEM プロジェクトアーキタイプ](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPA Editor](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [コアコンポーネント](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/introduction.html)
* [React JS](https://reactjs.org/)
* [Reactアプリの作成](https://create-react-app.dev/)

*チュートリアルの各パートの所要時間は 1～2 時間です。*

## 最新コード

チュートリアルのコードはすべて[GitHub](https://github.com/adobe/aem-guides-wknd-spa)にあります。

[最新のコードベース](https://github.com/adobe/aem-guides-wknd-spa/releases)は、ダウンロード可能なAEMパッケージとして入手できます。

## 前提条件

このチュートリアルを開始する前に、次が必要です。

* HTML、CSS、JavaScriptに関する基本的な知識
* [React](https://reactjs.org/tutorial/tutorial.html)に対する基本的な知識
* [AEM as aCloud ServiceSDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、 [AEM 6.5.4以降、](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65)  [AEM 6.4.8以降](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.](https://nodejs.org/ja/) jsand  [npm](https://www.npmjs.com/)

*必須ではありませんが、従来のAEM Sitesコンポーネントの開発に関する基本的な [知識を持つことは有益です](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)。*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、Mac OS環境で動作するAEM as aCloud ServiceSDK（[Visual Studio Code](https://code.visualstudio.com/)をIDEとして使用）を使用してキャプチャされます。 特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムとは独立している必要があります。

>[!NOTE]
>
> **AEM as a Cloud Service は初めてですか？** AEM as a  [Cloud ServiceSDKを使用したローカル開発環境のセットアップについては、次のガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5を初めて使用する場合** 次のガイドを参照し [て、ローカル開発環境を設定します](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 次の手順 {#next-steps}

何を待ってる?!チュートリアルを開始するには、「[SPA Editor Project](create-project.md)」の章に移動し、AEM Project Archetypeを使用してSPA Editor対応プロジェクトを生成する方法を学びます。

## 後方互換性 {#compatibility}

このチュートリアルのプロジェクトコードは、AEM as aCloud Service用に構築されています。 プロジェクトコードを&#x200B;**6.5.4+**&#x200B;と&#x200B;**6.4.8+**&#x200B;に対して後方互換性を保つために、チュートリアルのPOMファイルにいくつかの変更が加えられました。

[UberJar](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4**&#x200B;が依存関係として含まれています。

```xml
<!-- Adobe AEM 6.x Dependencies -->
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>uber-jar</artifactId>
    <version>6.4.4</version>
    <classifier>apis</classifier>
    <scope>provided</scope>
</dependency>
```

AEM 6.x環境を対象としてビルドを変更するために、`classic`という名前のMavenプロファイルが追加されました。

```xml
  <!-- AEM 6.x Profile to include Core Components-->
    <profile>
        <id>classic</id>
        <activation>
            <activeByDefault>false</activeByDefault>
        </activation>
        <build>
        ...
    </profile>
```

`classic`プロファイルは、デフォルトで無効になっています。 AEM 6.xでチュートリアルを実行する場合は、Mavenビルドの実行を指示されるたびに`classic`プロファイルを追加してください。例：

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM実装用の新しいプロジェクトを生成する場合は、常に最新バージョンの[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)を使用し、`aemVersion`を更新して、目的のバージョンのAEMをターゲットにします。
