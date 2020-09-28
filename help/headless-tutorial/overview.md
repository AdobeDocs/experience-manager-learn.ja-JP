---
title: AEMヘッドレス使用の手引き
description: AEMヘッドレスを使用してコンテンツを構築し、公開する方法を示す、エンドツーエンドのチュートリアルです。
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---


# AEMヘッドレス使用の手引き

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

ソースコードとコンテンツパッケージは、 [AEM Guides - WKND Mobile GitHub Project](https://github.com/adobe/aem-guides-wknd-mobile)で入手できます。

チュートリアルまたはコードに問題がある場合は、 [GitHubの問題を残してください](https://github.com/adobe/aem-guides-wknd-mobile/issues)。
