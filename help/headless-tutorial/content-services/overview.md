---
title: AEMヘッドレス — Content Services使用の手引き
description: AEM ヘッドレスを使用してコンテンツを構築および公開する方法を示す、エンドツーエンドのチュートリアルです。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 5%

---


# AEMヘッドレス — Content Services使用の手引き

AEMを使用してコンテンツを構築し、公開し、ネイティブモバイルアプリで使用され、ヘッドレスのCMSシナリオでコンテンツを利用する方法を説明するエンドツーエンドのチュートリアルです。

>[!VIDEO](https://video.tv.adobe.com/v/28315/?quality=12&learn=on)

このチュートリアルでは、AEM Content Servicesを使用して、イベント情報（音楽、パフォーマンス、アートなど）を表示するモバイルアプリの操作性を最大限に引き出す方法を説明します。 それはWKNDチームによってキュレーションされます。

このチュートリアルでは、次のトピックについて説明します。

* コンテンツフラグメントを使用したイベントを表すコンテンツの作成
* イベントデータをJSONとして公開するAEMサイトのテンプレートとページを使用してAEM Content Servicesエンドポイントを定義する
* AEM WCM Core Componentsを使用して、マーケターがJSONエンドポイントを作成できるようにする方法を学びます。
* モバイルアプリからAEM Content Services JSONを使用する
   * Androidを使用する理由は、このチュートリアルのすべてのユーザー（Windows、macOSおよびLinux）がネイティブのアプリケーションを実行する際に使用できるクロスプラットフォームエミュレーターがあるからです。

## GitHubプロジェクト

ソースコードとコンテンツパッケージは、[AEM GitHub Project](https://github.com/adobe/aem-guides-wknd-mobile)で入手できます。

チュートリアルまたはコードに問題がある場合は、[GitHubの問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)を残してください。
