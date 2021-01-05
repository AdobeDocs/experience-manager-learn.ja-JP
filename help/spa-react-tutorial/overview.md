---
title: AEM SPA Editor と React の使用の手引き
description: WKND SPAを搭載したAdobe Experience ManagerAEMで編集可能な、最初のReact Single Page Application(SPA)を作成します。 AEM SPAエディターでReact JSフレームワークを使用してSPAを作成する方法を説明します。 このマルチパートチュートリアルでは、架空のライフスタイルブランドであるWKNDに対するReactアプリケーションの実装について説明します。 このチュートリアルでは、SPAの作成とAEMとの統合の最後までを説明します。
sub-product: サイト
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5912
thumbnail: 5912-spa-react.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 18%

---


# AEM での React SPA の作成（チュートリアル） {#overview}

Adobe Experience Manager(AEM)の&#x200B;**SPAエディタ**&#x200B;機能を初めて使用する開発者向けに設計されたマルチパートチュートリアルをご利用いただき、ありがとうございます。 このチュートリアルでは、架空のライフスタイルブランドであるWKNDに対するReactアプリケーションの実装について説明します。 Reactアプリは、AEM SPA Editorでデプロイするように開発、設計されます。  Editorは、ReactコンポーネントをAEMコンポーネントにマッピングします。 完成したSPAは、AEMに導入され、従来のAEMのインライン編集ツールで動的に作成できます。

![最終的なSPAの実装](assets/wknd-spa-implementation.png)

*WKND SPAの実装*

##  について

このマルチパートチュートリアルの目標は、AEMのSPAエディタ機能を使用してReactアプリケーションを実装する方法を開発者に教えることです。 現実世界のシナリオでは、開発アクティビティはパーソナ別に分類され、多くの場合、**フロントエンド開発者**&#x200B;と&#x200B;**バックエンド開発者**&#x200B;が関与します。 AEM SPAエディタのプロジェクトに参加する開発者にとって、このチュートリアルを完了するのは有益なことだと思います。

このチュートリアルは、**AEMをCloud Service**&#x200B;として使用するように設計されており、**AEM 6.5.4+**&#x200B;および&#x200B;**AEM 6.4.8+**&#x200B;と下位互換性があります。 SPAは次を使用して実装されます。

* [Maven AEM プロジェクトアーキタイプ](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPAエディタ](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [コアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [React JS](https://reactjs.org/)
* [リアクトアプリを作成](https://create-react-app.dev/)

*チュートリアルの各パートの所要時間は 1～2 時間です。*

## 最新コード

チュートリアルコードはすべて[GitHub](https://github.com/adobe/aem-guides-wknd-spa)にあります。

[最新のコードベース](https://github.com/adobe/aem-guides-wknd-spa/releases)は、ダウンロード可能なAEMパッケージとして入手できます。

## 前提条件

このチュートリアルを開始する前に、次の情報が必要です。

* HTML、CSS、JavaScriptに関する基本的な知識
* [反応](https://reactjs.org/tutorial/tutorial.html)の基本的な知識
* [CLOUD SERVICESDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、AEM 6.5.4+ [または](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65)  [AEM 6.4.8+としてのAEM](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.](https://nodejs.org/ja/) jsand  [npm](https://www.npmjs.com/)

*AEM Sitesの伝統的な部品の [開発に関する基本的な理解を得ることは必須ではありませんが](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)、*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、Mac OS環境上で[Visual Studio Code](https://code.visualstudio.com/)をIDEとして実行するCloud ServiceSDKとしてAEMを使用してキャプチャされます。 コマンドとコードは、特に断りのない限り、ローカルのオペレーティングシステムとは独立している必要があります。

>[!NOTE]
>
> **AEM as a Cloud Service は初めてですか？** AEMをCloud ServiceSDKとして使用してローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5を初めて使用する場合** ローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 次の手順 {#next-steps}

何を待ってる！!「[SPA Editor Project](create-project.md)」の章に移動してチュートリアルを開始し、AEM Project Archetypeを使用してSPA Editor対応プロジェクトを生成する方法を学びます。

## 下位互換性{#compatibility}

このチュートリアルのプロジェクトコードは、AEM用にCloud Serviceとして構築されています。 プロジェクトコードを&#x200B;**6.5.4+**&#x200B;と&#x200B;**6.4.8+**&#x200B;に対して後方互換性を持たせるため、チュートリアルのPOMファイルにいくつかの変更が加えられました。

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

`classic`という名前のMavenプロファイルが追加され、ターゲットAEM 6.x環境にビルドを変更しました。

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

`classic`プロファイルはデフォルトで無効になっています。 AEM 6.xでチュートリアルを行う場合は、Mavenビルドの実行を指示された時に`classic`プロファイルを追加してください。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM実装用の新しいプロジェクトを生成する場合、必ず最新バージョンの[AEMプロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)を使用し、意図したバージョンのAEMをターゲットするように`aemVersion`を更新してください。
