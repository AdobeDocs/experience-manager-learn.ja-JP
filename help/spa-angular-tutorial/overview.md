---
title: AEM SPAエディタとAngularの使用の手引き
description: WKND SPAを使用し、AEMのAdobe Experience Managerで編集可能な最初のAngular Single Page Application(SPA)を作成します。 AEM SPAエディターでAngular JSフレームワークを使用してSPAを作成する方法を説明します。 このマルチパートチュートリアルでは、架空のライフスタイルブランドであるWKNDに対するAngularアプリケーションの実装について説明します。 このチュートリアルでは、SPAの作成とAEMとの統合の最後までを説明します。
sub-product: サイト
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5913
thumbnail: 5913-spa-angular.jpg
translation-type: tm+mt
source-git-commit: f568c991cd33c5c5349da32f505cff356a6ebfd2
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 12%

---


# AEMで最初のAngular SPAを作成する {#introduction}

Adobe Experience Manager(AEM)の **SPAエディタ** (SPA Editor)機能を初めて使用する開発者向けに設計されたマルチパートチュートリアルへようこそ。 このチュートリアルでは、架空のライフスタイルブランドであるWKNDに対するAngularアプリケーションの実装について説明します。 Angularアプリケーションは、AngularコンポーネントをAEMコンポーネントにマッピングするAEM SPA Editorと共にデプロイするように開発および設計されます。 完成したSPAはAEMに展開され、AEMの従来のインライン編集ツールで動的に作成できます。

![最終的なSPA実装](assets/wknd-spa-implementation.png)

*WKND SPAの実装*

## バージョン情報

このマルチパートチュートリアルの目標は、AEMのSPAエディタ機能を使用するAngularアプリケーションの実装方法を開発者に教えることです。 In a real-world scenario the development activities are broken down by persona, often involving a **Front End developer** and a **Back End developer**. AEM SPAエディタのプロジェクトに参加する開発者にとって、このチュートリアルを完了するのは有益なことだと思います。

このチュートリアルは、 **AEMをCloud Serviceとして使用するように設計されており、** AEM 6.5.4+ **および** AEM 6.4.8+と下位互換性があり ****&#x200B;ます。 SPAは次を使用して実装されます。

* [Maven AEM プロジェクトアーキタイプ](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/developing/archetype/overview.html)
* [AEM SPAエディタ](https://docs.adobe.com/content/help/en/experience-manager-65/developing/headless/spas/spa-walkthrough.html#content-editing-experience-with-spa)
* [コアコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)
* [角](https://angular.io/)

*チュートリアルの各パートの所要時間は 1～2 時間です。*

## 最新コード

チュートリアルのコードはすべて [GitHubにあります](https://github.com/adobe/aem-guides-wknd-spa)。

最 [新のコードベース](https://github.com/adobe/aem-guides-wknd-spa/releases) は、AEM Packagesをダウンロードして入手できます。

## 前提条件

このチュートリアルを開始する前に、次の情報が必要です。

* HTML、CSS、JavaScriptに関する基本的な知識
* Angularに関する基本的な知識 [](https://angular.io/)
* [CLOUD SERVICESDK](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html#download-the-aem-as-a-cloud-service-sdk)、 [AEM 6.5.4+](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#65) または [AEM 6.4.8+としてのAEM](https://helpx.adobe.com/experience-manager/aem-releases-updates.html#64)
* [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
* [Apache Maven](https://maven.apache.org/) （3.3.9以降）
* [Node.js](https://nodejs.org/en/) と [npm](https://www.npmjs.com/)

*AEM Sitesの伝統的な部品の[開発に関する基本的な理解を得ることは必須ではありませんが](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)、*

## ローカル開発環境 {#local-dev-environment}

このチュートリアルを完了するには、ローカルの開発環境が必要です。スクリーンショットとビデオは、 [Visual StudioコードをIDEとして使用し、Mac OS環境上で実行するCloud ServiceSDKとしてAEMを使用してキャプチャされ](https://code.visualstudio.com/) ます。 コマンドとコードは、特に断りのない限り、ローカルのオペレーティングシステムとは独立している必要があります。

>[!NOTE]
>
> **AEMを初めてCloud Serviceに？** AEMをCloud ServiceSDKとして使用してローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/en/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html)。
>
> **AEM 6.5を初めて使用する場合** ローカル開発環境を設定するには、 [次のガイドを参照してください](https://docs.adobe.com/content/help/en/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html)。

## 次の手順 {#next-steps}

何を待ってる！!「 [SPAエディタのプロジェクト](create-project.md) 」の章に進み、AEMプロジェクトのアーキタイプを使用してSPAエディタが有効なプロジェクトを生成する方法を学び、チュートリアルを開始します。

## Backward compatibility {#compatibility}

このチュートリアルのプロジェクトコードは、AEM用にCloud Serviceとして構築されています。 プロジェクトコードを **6.5.4+と6.4.8+で下位互換性を持たせるために、いくつかの変更が行われました****** 。

UberJar [](https://docs.adobe.com/content/help/en/experience-manager-65/developing/devtools/ht-projects-maven.html#what-is-the-uberjar) v6.4.4 **** は、次の依存関係に含まれています。

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

ターゲットAEM 6.xのプロファイルにビルドを変更するため `classic` に、次の名前のMaven環境が追加されました。

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

The `classic` profile is disabled by default. AEM 6.xでのチュートリアルに従う場合は、Mavenビルドの実行を指示されたときに `classic` プロファイルを追加してください。

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

AEM実装用の新しいプロジェクトを生成する場合は、常に最新バージョンの [AEMプロジェクトアーキタイプを使用し](https://github.com/adobe/aem-project-archetype) 、意図したバージョンのAEM `aemVersion` ターゲットに更新します。
