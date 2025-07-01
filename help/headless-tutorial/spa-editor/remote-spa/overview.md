---
title: SPA エディターとリモート SPA の基本を学ぶ - 概要
description: 開発者向けのマルチパートチュートリアルへようこそ。ここでは、AEM SPA エディターを使用して編集可能な AEM コンテンツで既存のリモート SPA を拡張する方法を見ていきます。
topic: Headless, SPA, Development
feature: SPA Editor, Core Components, APIs, Developing
role: Developer, Architect
level: Beginner
jira: KT-7630
thumbnail: 333272.jpeg
last-substantial-update: 2022-11-11T00:00:00Z
doc-type: Tutorial
exl-id: c5f933eb-c409-41dc-bb6a-6b2220dfbb47
duration: 294
hide: true
source-git-commit: 5b008419d0463e4eaa1d19c9fe86de94cba5cb9a
workflow-type: ht
source-wordcount: '571'
ht-degree: 100%

---

# はじめに

{{edge-delivery-services}}

開発者向けのマルチパートチュートリアルへようこそ。ここでは、AEM SPA エディターを使用して編集可能な AEM コンテンツで既存の React ベース（Next.js）のリモート SPA を拡張する方法を見ていきます。

このチュートリアルは、AEM の GraphQL API を介して AEM コンテンツフラグメントコンテンツを使用する React アプリである [WKND GraphQL アプリ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ja)をベースにしていますが、SPA コンテンツのコンテキスト内オーサリングは提供していません。

>[!VIDEO](https://video.tv.adobe.com/v/3444848?quality=12&learn=on&captions=jpn)

## このチュートリアルについて

このチュートリアルでは、AEM のコンテキスト外で動作している SPA やリモート SPA を、AEM でオーサリングしたコンテンツを使用および配信するように更新する方法について説明します。

このチュートリアルのほとんどのアクティビティでは JavaScript による開発に重点が置かれていますが、AEM を中心とする非常に重要な側面についても扱います。こうした側面としては、AEM でコンテンツをオーサリングおよび保存する場所の定義や、AEM ページへの SPA ルートのマッピングなどがあります。

このチュートリアルは **AEM as a Cloud Service** で動作するように作られており、次の 2 つのプロジェクトで構成されています。

1. __AEM プロジェクト__&#x200B;には、AEM にデプロイする必要がある設定とコンテンツが含まれています。
1. __WKND アプリ__&#x200B;プロジェクトは、AEM の SPA エディターと統合する SPA です。

## 最新のコード

+ このチュートリアルのコードは、[GitHub](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/remote-spa-tutorial) の `remote-spa-tutorial` フォルダーにあるコードを出発点としています。

## 前提条件

このチュートリアルでは、次の項目が必要です。

+ [AEM as a Cloud Service の SDK](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/aem-runtime.html?lang=ja)
+ [Node.js v18](https://nodejs.org/ja/)
+ [Java™ 11](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html)
+ [Maven 3.6+](https://maven.apache.org/)
+ [Git](https://git-scm.com/downloads)
+ [aem-guides-wknd.all-2.1.0.zip 以降](https://github.com/adobe/aem-guides-wknd/releases)
+ [aem-guides-wknd-graphql ソースコード](https://github.com/adobe/aem-guides-wknd-graphql/tree/main)

このチュートリアルでは、次を前提としています。

+ IDE としての [Microsoft® Visual Studio Code](https://visualstudio.microsoft.com/)
+ `~/Code/aem-guides-wknd-graphql/remote-spa-tutorial` の作業ディレクトリ
+ AEM SDK を `http://localhost:4502` でオーサーサービスとして実行
+ `admin`パスワードを持つローカルアカウント`admin`での AEM SDK の実行
+ SPA が `http://localhost:3000` で動作していること

>[!NOTE]
>
> **ローカル開発環境のセットアップに関するサポートが必要な場合は、**[AEM as a Cloud Service SDK を使用するローカル開発環境のセットアップに関するガイド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)を参照してください。

## &#x200B;1. SPA エディターに対応する AEM の設定

SPA を AEM SPA エディターと統合するには、AEM の設定が必要です。これらの設定は、AEM プロジェクトを通じて管理およびデプロイされます。この章では、必要な設定とその定義方法について説明します。

+ [SPA エディター向けの AEM の設定方法についてはこちらを参照](./aem-configure.md)

## &#x200B;2. SPA のブートストラップ

AEM SPA エディターで SPA をオーサリングコンテキストに統合するには、SPA に対していくつかの追加を行う必要があります。

+ [AEM SPA エディター向けに SPA をブートストラップする方法についてはこちらを参照](./spa-bootstrap.md)

## &#x200B;3. 編集可能な固定コンポーネント

まず、編集可能な「固定コンポーネント」を SPA に追加する方法を調べます。この例では、特定の編集可能なコンポーネントを SPA に配置する方法を示しています。作成者はコンポーネントのコンテンツを変更できますが、コンポーネントを削除したり、コンポーネントの配置や位置またはサイズを変更したりすることはできません。

+ [編集可能な固定コンポーネントについてはこちらを参照](./spa-fixed-component.md)

## &#x200B;4. 編集可能なコンテナコンポーネント

次に、編集可能な「コンテナコンポーネント」を SPA に追加する方法を説明します。 この例では、SPA にコンテナコンポーネントを配置する方法を示しています。 コンテナコンポーネントを使用すると、作成者は許可されたコンポーネントをその中に配置したり、コンポーネントのレイアウトを調整したりできます。

+ [編集可能なコンテナコンポーネントについて学ぶ](./spa-container-component.md)

## &#x200B;5. 動的ルートと編集可能コンポーネント

最後に、動的ルート（ルートのパラメータに基づいて異なるコンテンツを表示するルート）に関する前の章で説明した概念を活用します。 この例では、AEM SPA エディターを使用して、プログラムによって駆動、生成されるルートのコンテンツを作成する方法を示します。

+ [動的ルートと編集可能なコンポーネントについて学ぶ](./spa-dynamic-routes.md)

## その他のリソース

+ [AEM SPA React 編集可能コンポーネント](https://www.npmjs.com/package/@adobe/aem-react-editable-components)
