---
title: AEM as a Cloud Service のローカル開発環境
description: Adobe Experience Manager（AEM）ローカル開発環境の概要
feature: Developer Tools
version: Cloud Service
doc-type: article
jira: KT-3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
duration: 835
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 100%

---

# ローカル開発環境の設定 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概要"
>abstract="AEM as a Cloud Service のローカル開発環境の設定には、AEM Projects の開発、ビルド、コンパイルに必要な開発ツールや、Adobe Cloud Manager を使用して AEM as a Cloud Service に新しい機能をデプロイする前に、開発者が新しい機能をローカルですばやく検証できるローカルランタイムが含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ja" text="開発ガイドライン"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=ja" text="開発の基本"

ローカル開発環境の設定 - このチュートリアルでは、AEM as a Cloud Service SDK を使用して Adobe Experience Manager（AEM）用のローカル開発環境を設定する手順について説明します。AEM Projects の開発、ビルド、コンパイルに必要な開発ツールや、ローカルランタイムが含まれているので、開発者は、Adobe Cloud Manager を使用して AEM as a Cloud Service に新しい機能をデプロイする前に、新しい機能をローカルランタイムですばやく検証できます。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEM as a Cloud Service ローカル開発環境のテクノロジースタック](./assets/overview/aem-sdk-technology-stack.png)

AEM のローカル開発環境は、次の 3 つの論理グループに分割できます。

+ この __AEM プロジェクト__&#x200B;には、カスタム AEM アプリケーションのカスタムコード、設定およびコンテンツが含まれます。
+ この&#x200B;__ローカル AEM ランタイム__&#x200B;は、AEM オーサーサービスとパブリッシュサービスのローカルバージョンを実行します。
+ この&#x200B;__ローカル Dispatcher ランタイム__&#x200B;は、Apache HTTP web サーバーおよび Dispatcher のローカルバージョンを実行します。

このチュートリアルでは、上の図で強調表示されている項目をインストールして設定し、AEM 開発用の安定したローカル開発環境を提供する方法について説明します。

## ファイルシステムの構成

このチュートリアルでは、AEM as a Cloud Service SDK アーティファクトと AEM プロジェクトコードの場所を次のように設定しました。

+ `~/aem-sdk` は、AEM as a Cloud Service SDK が提供する様々なツールを含む組織フォルダー
+ `~/aem-sdk/author` は、AEM オーサーサービスを含む
+ `~/aem-sdk/publish` は、AEM パブリッシュサービスを含む
+ `~/aem-sdk/dispatcher` は Dispatcher ツールを含む
+ `~/code/<project name>` は、カスタム AEM プロジェクトソースコードを含む

メモ：`~` は、ユーザーのディレクトリの略記法です。Windows では、`%HOMEPATH%` に相当します。

## AEM プロジェクト用開発ツール

AEM プロジェクトは、Cloud Manager を介して AEM as a Cloud Service にデプロイされるコード、設定およびコンテンツを含むカスタムコードベースです。ベースラインプロジェクト構造は、[AEM プロジェクト Maven アーキタイプ](https://github.com/adobe/aem-project-archetype)です。

チュートリアルのこの節では、次の方法について説明します。

+ [!DNL Java] のインストール 
+ [!DNL Node.js] （および npm）のインストール
+ [!DNL Maven] のインストール 
+ [!DNL Git] のインストール 

[AEM プロジェクト用の開発ツールの設定](./development-tools.md)

## ローカル AEM ランタイム

AEM as a Cloud Service SDK は、AEM のローカルバージョンを実行する [!DNL QuickStart Jar] を提供します。この [!DNL QuickStart Jar] を使用して、AEM オーサーサービスまたは AEM パブリッシュサービスをローカルで実行できます。なお、[!DNL QuickStart Jar] はローカルでの開発環境を提供しますが、AEM as a Cloud Service で利用できるすべての機能が [!DNL QuickStart Jar] に含まれるわけではありません。

チュートリアルのこの節では、次の方法について説明します。

+ [!DNL Java] のインストール 
+ AEM SDK をダウンロード
+ [!DNL AEM Author Service] を実行する
+ [!DNL AEM Publish Service] を実行する

[ローカル AEM ランタイムの設定](./aem-runtime.md)

## ローカル [!DNL Dispatcher] ランタイム

AEM as a Cloud Service SDK の Dispatcher ツールには、ローカル [!DNL Dispatcher] ランタイムの設定に必要な機能がすべて備わっています。[!DNL Dispatcher] ツールは [!DNL Docker] ベースで、[!DNL Apache HTTP] web サーバーと [!DNL Dispatcher] 設定ファイルを互換性のある形式にトランスパイルし、[!DNL Docker] コンテナで動作する [!DNL Dispatcher] にデプロイするコマンドラインツールを提供します。

チュートリアルのこの節では、次の方法について説明します。

+ AEM SDK をダウンロード
+ [!DNL Dispatcher] ツールをインストール
+ ローカル [!DNL Dispatcher] ランタイムを実行

[ローカル  [!DNL Dispatcher]  ランタイムを設定](./dispatcher-tools.md)
