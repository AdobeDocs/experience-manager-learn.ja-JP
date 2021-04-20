---
title: ドキュメントフラグメントの作成
description: 'これは、最初の対話型通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 この部分では、受信者名とアドレスを保持するドキュメントフラグメントを作成します。 '
seo-description: 'これは、最初の対話型通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 この部分では、受信者名とアドレスを保持するドキュメントフラグメントを作成します。 '
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 1%

---


# ドキュメントフラグメントの作成

この部分では、受信者名とアドレスを保持するドキュメントフラグメントを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

ドキュメントフラグメントは、対話型通信ドキュメントのテキストコンテンツを保持します。 このテキストコンテンツは、静的テキストにすることも、基になるデータモデル要素の値から挿入することもできます。 例えば、**様&#x200B;_{name}_**様（Dearはスタティックテキスト、nameはフォームデータモデル要素名）。 実行時に、これはname要素の値に応じて&#x200B;**Gloria Rios様**様または&#x200B;**John Jacobs様**様に解決されます。

リッチテキストエディターは、ビジネスユーザーがテキストを作成し、フォームデータ要素を挿入できる直感的なエディターです。 ドキュメントフラグメントエディターでは、テキストの書式設定、フォントの種類とスタイルの指定、特殊文字の挿入、ハイパーリンクの作成を行うことができます。

また、この[ビデオ](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)で示すように、ドキュメントフラグメントエディターでは、テキストにインライン条件を挿入することもできます。

>[!NOTE]
>
>ドキュメントフラグメントに挿入するForm Data Model要素が、ルート要素の子孫であることを確認します。 例えば、この使用例では、選択したユーザー・オブジェクトの要素がbalancesオブジェクトの子であることを確認します

