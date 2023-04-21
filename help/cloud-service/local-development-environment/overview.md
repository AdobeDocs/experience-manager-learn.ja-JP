---
title: AEM as a Cloud Service のローカル開発環境
description: Adobe Experience Manager(AEM)ローカル開発環境の概要
feature: Developer Tools
version: Cloud Service
doc-type: article
kt: 3290
thumbnail: 32565.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 8b12f34c-be98-4f47-853c-411bb601990c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 16%

---

# ローカル開発環境の設定 {#local-development-environment-set-up}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概要"
>abstract="AEM as a Cloud Service のローカル開発環境には、AEM プロジェクトの開発、ビルド、コンパイルに必要な開発ツールや、Adobe Cloud Manager を介して AEM as a Cloud Service に新機能をデプロイする前に、ローカルで迅速に検証できるローカルランタイムが含まれています。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ja" text="開発ガイドライン"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html?lang=ja" text="開発の基本"

このチュートリアルでは、AEMas a Cloud ServiceSDK を使用したAdobe Experience Manager(AEM) のローカル開発環境のセットアップに関する手順を説明します。 AEM Projects の開発、ビルド、コンパイルに必要な開発ツールや、ローカルの実行時間が含まれているので、開発者は、Cloud Manager を使用してAEM as a Cloud Serviceに新しい機能をデプロイする前に、新しい機能をローカルですばやく検証できます。

>[!VIDEO](https://video.tv.adobe.com/v/32565?quality=12&learn=on)

![AEMas a Cloud Serviceローカル開発環境のテクノロジースタック](./assets/overview/aem-sdk-technology-stack.png)

AEMのローカル開発環境は、次の 3 つの論理グループに分割できます。

+ この __AEM Project__ には、カスタムAEMアプリケーションのカスタムコード、設定およびコンテンツが含まれます。
+ この __Local AEM Runtime__ :AEM オーサーサービスとパブリッシュサービスのローカルバージョンを実行します。
+ この __ローカル Dispatcher ランタイム__ Apache HTTP Web サーバーおよび Dispatcher のローカルバージョンを実行する

このチュートリアルでは、上の図で強調表示されている項目をインストールして設定し、AEM開発用の安定したローカル開発環境を提供する方法について説明します。

## ファイルシステムの構成

このチュートリアルでは、次のように、AEMas a Cloud ServiceSDK アーティファクトとAEMプロジェクトコードの場所を確立しました。

+ `~/aem-sdk` は、AEM as a Cloud Service SDK が提供する様々なツールを含む組織フォルダーです
+ `~/aem-sdk/author` には、AEM オーサーサービスが含まれています
+ `~/aem-sdk/publish` には、AEM パブリッシュサービスが含まれています
+ `~/aem-sdk/dispatcher` Dispatcher ツールを含む
+ `~/code/<project name>` には、カスタムAEM Project ソースコードが含まれています

注意： `~` は、ユーザーのディレクトリの略記法です。 Windows の場合、これは `%HOMEPATH%`;

## AEM Projects 用開発ツール

AEMプロジェクトは、Cloud Manager を介してAEM as a Cloud Serviceにデプロイされるコード、設定およびコンテンツを含むカスタムコードベースです。 ベースラインプロジェクト構造は、 [AEM Project Maven アーキタイプ](https://github.com/adobe/aem-project-archetype).

このチュートリアルのこの節では、次の方法について説明します。

+ [!DNL Java] のインストール 
+ インストール [!DNL Node.js] （および npm）
+ [!DNL Maven] のインストール 
+ [!DNL Git] のインストール 

[AEM Projects 用開発ツールの設定](./development-tools.md)

## ローカル AEM ランタイム

AEMas a Cloud ServiceSDK は、 [!DNL QuickStart Jar] AEMのローカルバージョンを実行する この [!DNL QuickStart Jar] を使用して、AEM オーサーサービスまたは AEM パブリッシュサービスをローカルで実行できます。 なお、 [!DNL QuickStart Jar] はローカル開発エクスペリエンスを提供しますが、AEM as a Cloud Serviceで使用できるすべての機能が [!DNL QuickStart Jar].

このチュートリアルのこの節では、次の方法について説明します。

+ [!DNL Java] のインストール 
+ AEM SDK のダウンロード
+ を実行します。 [!DNL AEM Author Service]
+ を実行します。 [!DNL AEM Publish Service]

[ローカルAEMランタイムの設定](./aem-runtime.md)

## ローカル [!DNL Dispatcher] ランタイム

AEM as a Cloud Service SDK の Dispatcher ツールは、ローカルの設定に必要なすべてを提供します [!DNL Dispatcher] ランタイム。 [!DNL Dispatcher] ツールは [!DNL Docker]-based に設定され、変換するコマンドラインツールを提供します。 [!DNL Apache HTTP] Web サーバーおよび [!DNL Dispatcher] 設定ファイルを互換性のある形式に変換し、次の場所にデプロイします。 [!DNL Dispatcher] 実行， [!DNL Docker] コンテナ。

このチュートリアルのこの節では、次の方法について説明します。

+ AEM SDK のダウンロード
+ インストール [!DNL Dispatcher] ツール
+ ローカルで実行 [!DNL Dispatcher] runtime

[ローカルの [!DNL Dispatcher] ランタイム](./dispatcher-tools.md)
