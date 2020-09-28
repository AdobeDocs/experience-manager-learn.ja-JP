---
title: Cloud ServiceとしてのAEMのローカル開発環境
description: Adobe Experience Manager(AEM)現地開発環境の概要
feature: null
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
translation-type: tm+mt
source-git-commit: 69c1767098cc9da8ec0ae2bd83d25417d330f393
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 1%

---


# ローカル開発環境の設定

このチュートリアルでは、AEMをCloud ServiceSDKとして使用して、Adobe Experience Manager(AEM)用のローカル開発環境をセットアップする手順を説明します。 AEMプロジェクトの開発、構築、コンパイルに必要な開発ツールや、ローカルの実行時に開発者が新機能をローカルで検証し、AEMにCloud ServiceとしてAdobeCloud Managerを介して展開できるようにするツールが含まれます。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![CLOUD SERVICEのローカル開発環境テクノロジスタックとしてのAEM](./assets/overview/aem-sdk-technology-stack.png)

AEMのローカル開発環境は、次の3つの論理グループに分類できます。

+ AEM __プロジェクト__ には、カスタムAEMアプリケーションのカスタムコード、設定、およびコンテンツが含まれます。
+ AEM Authorサービスと __Publishサービスのローカルバージョンをローカルで実行するLocal AEM Runtime__ 。
+ Apache HTTP Web Serverおよび __Dispatcherのローカルバージョンを実行するLocal Dispatcher Runtime__ 。

このチュートリアルでは、上の図で強調表示されている項目をインストールして設定する方法を紹介し、AEMの開発に安定したローカル開発環境を提供します。

## ファイルシステムの構成

このチュートリアルでは、AEMの場所をCloud ServiceSDKアーティファクトとAEMプロジェクトコードとして次のように確立しました。

+ `~/aem-sdk` は、AEMがCloud ServiceSDKとして提供する様々なツールを含む組織フォルダーです
+ `~/aem-sdk/author` にAEM Author Serviceが含まれる
+ `~/aem-sdk/publish` にAEM発行サービスが含まれる
+ `~/aem-sdk/dispatcher` には、ディスパッチャーツールが含まれます。
+ `~/code/<project name>` カスタムAEMプロジェクトソースコードが含まれます。

は、ユーザーのディレクトリ `~` の略記法です。 Windowsでは、次と同じで `%HOMEPATH%`す。

## AEMプロジェクト用開発ツール

AEMプロジェクトは、Cloud Managerを介してAEMにCloud Serviceとしてデプロイされるコード、設定およびコンテンツを含むカスタムコードベースです。 基準プロジェクト構造は、 [AEM Project Mavenアーキタイプを介して生成され](https://github.com/adobe/aem-project-archetype)ます。

チュートリアルのこの節では、次の方法を示します。

+ インストール [!DNL Java]
+ インストール [!DNL Node.js] （およびnpm）
+ インストール [!DNL Maven]
+ インストール [!DNL Git]

[AEMプロジェクト用の開発ツールの設定](./development-tools.md)

## ローカルAEMランタイム

Cloud ServiceSDKとしてのAEMは、ローカルバージョンのAEM [!DNL QuickStart Jar] を実行するサービスを提供します。 は、AEM Author ServiceまたはAEM Publish Serviceをローカルで実行するために使用 [!DNL QuickStart Jar] できます。 にはローカルの開発経験が [!DNL QuickStart Jar] ありますが、AEMでCloud Serviceとして利用できる機能の一部はに含まれていません [!DNL QuickStart Jar]。

チュートリアルのこの節では、次の方法を示します。

+ インストール [!DNL Java]
+ AEM SDKのダウンロード
+ JavaScriptコードの [!DNL AEM Author Service]
+ JavaScriptコードの [!DNL AEM Publish Service]

[ローカルAEMランタイムの設定](./aem-runtime.md)

## ローカル [!DNL Dispatcher] ランタイム

AEMは、Cloud ServiceSDKのディスパッチャーツールとして、ローカル [!DNL Dispatcher] ランタイムの設定に必要なすべてを提供します。 [!DNL Dispatcher] ツールは [!DNL Docker]ベースで、Webサーバーと [!DNL Apache HTTP] 設定ファイルを互換性のある形式に変換し、 [!DNL Dispatcher] コンテナでの [!DNL Dispatcher][!DNL Docker] 実行に導入するためのコマンドラインツールを提供します。

チュートリアルのこの節では、次の方法を示します。

+ AEM SDKのダウンロード
+ ツールのインスト [!DNL Dispatcher] ール
+ ローカル [!DNL Dispatcher] ランタイムの実行

[LocalRuntimeの設定 [!DNL Dispatcher] ](./dispatcher-tools.md)
