---
title: AEM as aCloud Serviceのローカル開発環境
description: Adobe Experience Manager(AEM)ローカル開発環境の概要
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: article
activity: troubleshoot
audience: developer
kt: 3290
thumbnail: 32565.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: 9a78cbdb5fd35e4aa7169382494dd014aa8098e9
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 3%

---


# ローカル開発環境の設定

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_overview"
>title="概要"
>abstract="AEM as aCloud Service用のローカル開発環境の設定には、AEMプロジェクトの開発、構築、コンパイルに必要な開発ツールのほか、新機能をAdobeCloud Managerを使用してAEMにデプロイする前に、ローカルですばやく検証できるローカルの実行時間が含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html?lang=ja" text="開発ガイドライン"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/aem-sdk.html" text="開発の基本"

このチュートリアルでは、AEM as aCloud ServiceSDKを使用したAdobe Experience Manager(AEM)のローカル開発環境のセットアップに関する手順を説明します。 AEMプロジェクトの開発、構築、コンパイルに必要な開発ツールと、ローカルの実行時間が含まれているので、AdobeCloud Managerを使用してAEMに新機能をCloud Serviceとしてデプロイする前に、新機能をローカルですばやく検証できます。

>[!VIDEO](https://video.tv.adobe.com/v/32565/?quality=12&learn=on)

![AEM as a Cloud Service環境ローカル開発テクノロジースタック](./assets/overview/aem-sdk-technology-stack.png)

AEMのローカル開発環境は、次の3つの論理グループに分割できます。

+ __AEMプロジェクト__&#x200B;には、カスタムAEMアプリケーションのカスタムコード、設定およびコンテンツが含まれます。
+ __Local AEM Runtime__:AEMオーサーサービスとパブリッシュサービスのローカルバージョンをローカルで実行します。
+ __Local Dispatcher Runtime__（Apache HTTP WebサーバーおよびDispatcherのローカルバージョンを実行）。

このチュートリアルでは、上の図で強調表示されている項目をインストールして設定し、AEM開発用の安定したローカル開発環境を提供する方法について説明します。

## ファイル・システム構成

このチュートリアルでは、AEM as a Cloud ServiceSDKアーティファクトとAEM Projectコードの場所を次のように確立しました。

+ `~/aem-sdk` は、AEM as a Cloud Service SDKが提供する様々なツールを含む組織フォルダーです
+ `~/aem-sdk/author` AEMオーサーサービスを含む
+ `~/aem-sdk/publish` AEMパブリッシュサービスを含む
+ `~/aem-sdk/dispatcher` Dispatcherツールを含む
+ `~/code/<project name>` カスタムAEM Projectソースコードが含まれる

`~`は、ユーザーのディレクトリの略記法です。 Windowsの場合、これは`%HOMEPATH%`と同じです。

## AEM Projectsの開発ツール

AEMプロジェクトは、Cloud Managerを通じてAEMにCloud Serviceとしてデプロイされるコード、設定およびコンテンツを含むカスタムコードベースです。 ベースラインプロジェクト構造は、[AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype)を使用して生成されます。

このチュートリアルの節では、次の方法について説明します。

+ インストール [!DNL Java]
+ [!DNL Node.js] （およびnpm）をインストールします。
+ インストール [!DNL Maven]
+ インストール [!DNL Git]

[AEM Projects用開発ツールの設定](./development-tools.md)

## Local AEM Runtime

AEM as aCloud ServiceSDKは、AEMのローカルバージョンを実行する[!DNL QuickStart Jar]を提供します。 [!DNL QuickStart Jar]は、AEMオーサーサービスまたはAEMパブリッシュサービスをローカルで実行するために使用できます。 [!DNL QuickStart Jar]はローカル開発体験を提供しますが、AEM as aCloud Serviceとして使用できるすべての機能が[!DNL QuickStart Jar]に含まれるわけではありません。

このチュートリアルの節では、次の方法について説明します。

+ インストール [!DNL Java]
+ AEM SDKのダウンロード
+ [!DNL AEM Author Service]
+ [!DNL AEM Publish Service]

[ローカルAEMランタイムの設定](./aem-runtime.md)

## ローカルの[!DNL Dispatcher]ランタイム

AEM as a Dispatcher SDKのDispatcherツールは、ローカルの[!DNL Dispatcher]ランタイムを設定するために必要なすべての機能を提供します。 [!DNL Dispatcher] ツールはベ [!DNL Docker]ースで、 [!DNL Apache HTTP] Webサーバーと設定ファイルを互換性のある形式で転送 [!DNL Dispatcher] し、コンテナ内で実行中にデプロイするためのコマンドラインツ [!DNL Dispatcher] ールを提供 [!DNL Docker] します。

このチュートリアルの節では、次の方法について説明します。

+ AEM SDKのダウンロード
+ [!DNL Dispatcher]ツールをインストールします
+ ローカルの[!DNL Dispatcher]ランタイムを実行します

[ローカル [!DNL Dispatcher] ランタイムの設定](./dispatcher-tools.md)
