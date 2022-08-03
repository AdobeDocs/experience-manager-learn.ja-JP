---
title: AEM SPA Editor と Angular の使用の手引き
description: WKND SPA を使用して、Adobe Experience Manager（AEM）で編集可能な最初の Angular 単一ページアプリケーション（SPA）を作成します。
sub-product: sites
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
feature: SPA Editor
topic: SPA
role: Developer
level: Beginner
exl-id: f2cf4063-0b08-4b4f-91e6-70e5a148f931
source-git-commit: 825124bc6c3be10e6822fb5fb8bd9645d242da76
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# AEM での Angular SPA の作成（チュートリアル） {#introduction}

このたびは、 **SPA Editor** Adobe Experience Manager(AEM) の機能を使用できます。 このチュートリアルでは、架空のライフスタイルブランドである WKND 向けのAngularアプリケーションの実装について説明します。 angularアプリは、AEM SPA Editor を使用してデプロイされるように開発および設計されます。このエディターは、AngularコンポーネントをAEMコンポーネントにマッピングします。 AEMにデプロイされた完成したSPAは、AEMの従来のインライン編集ツールを使用して動的にオーサリングできます。

![最終的なSPAの実装](assets/wknd-spa-implementation.png)

*WKND SPAの実装*

## について

この複数パートから成るチュートリアルの目的は、AEMのSPAエディター機能と連携するAngularアプリケーションを実装する方法を開発者に教えることです。 実際のシナリオでは、開発アクティビティは、多くの場合、 **フロントエンド開発者** および **バックエンド開発者**. AEM SPA Editor プロジェクトに参加するすべての開発者が、このチュートリアルを完了できると便利です。

このチュートリアルは、 **AEMas a Cloud Service** との後方互換性がある **AEM 6.5.4 以降** および **AEM 6.4.8 以降**. SPAは、次を使用して実装されます。

* [Maven AEM プロジェクトアーキタイプ](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/developing/archetype/overview.html?lang=ja)
* [AEM SPA Editor](https://experienceleague.adobe.com/docs/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [Angular](https://angular.io/)

*チュートリアルの各パートの所要時間は 1～2 時間です。*

## 最新のコード

チュートリアルコードはすべて、 [GitHub](https://github.com/adobe/aem-guides-wknd-spa).

この [最新のコードベース](https://github.com/adobe/aem-guides-wknd-spa/releases) は、ダウンロード可能なAEMパッケージとして入手できます。

## 前提条件

このチュートリアルを開始する前に、次の手順を実行する必要があります。

* HTML、CSS、JavaScript に関する基本的な知識
* ～に対する基本的な知識 [Angular](https://angular.io/)
* [AEMas a Cloud ServiceSDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja#download-the-aem-as-a-cloud-service-sdk), [AEM 6.5.4 以降](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) または [AEM 6.4.8 以降](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/)（3.3.9 以降）
* [Node.js](https://nodejs.org/ja/) および [npm](https://www.npmjs.com/)

*必須ではありませんが、基本的な理解を持つことは有益です [従来のAEM Sitesコンポーネントの開発](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ja).*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、Mac OS 環境で動作しているAEMas a Cloud ServiceSDK を使用して、 [Visual Studio Code](https://code.visualstudio.com/) を IDE として使用します。 特に断りのない限り、コマンドとコードはローカルのオペレーティングシステムから独立している必要があります。

>[!NOTE]
>
> **AEM as a Cloud Service は初めてですか？** 以下を確認します。 [AEM as a Cloud Service SDK を使用したローカル開発環境の設定に関する以下のガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja).
>
> **AEM 6.5 を初めて使用する場合** 以下を確認します。 [次のガイドでは、ローカル開発環境を設定します](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja).

## 次の手順 {#next-steps}

何を待ってる?!チュートリアルを開始するには、 [SPA Editor Project](create-project.md) この章では、AEMプロジェクトアーキタイプを使用してSPA Editor が有効なプロジェクトを生成する方法について説明します。

## 後方互換性 {#compatibility}

このチュートリアルのプロジェクトコードは、AEM as a Cloud Service用に構築されています。 プロジェクトコードをに後方互換性を持たせるため **6.5.4 以上** および **6.4.8 以上** いくつかの変更が加えられました。

この [UberJar](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) **v6.4.4** は依存関係として含まれています：

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

という名前の追加の Maven プロファイル `classic` を追加して、target AEM 6.x 環境のビルドを変更しました。

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

この `classic` プロファイルはデフォルトで無効になっています。 AEM 6.x でチュートリアルに従う場合は、 `classic` Maven ビルドの実行を指示された場合は常に、次のプロファイルを使用します。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM実装用の新しいプロジェクトを生成する場合、常に最新バージョンの [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) を更新し、 `aemVersion` をクリックして、AEMの意図したバージョンをターゲットにします。
