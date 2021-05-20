---
title: ドキュメントフラグメントの作成
description: 'これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 ここでは、受信者の名前と住所を格納するドキュメントフラグメントを作成します。 '
seo-description: 'これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 ここでは、受信者の名前と住所を格納するドキュメントフラグメントを作成します。 '
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 1%

---


# ドキュメントフラグメントの作成

ここでは、受信者の名前と住所を格納するドキュメントフラグメントを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

ドキュメントフラグメントには、インタラクティブ通信ドキュメントのテキストコンテンツが格納されます。 このテキストコンテンツは、静的テキストにすることも、基になるデータモデル要素の値から挿入することもできます。 例えば、**親&#x200B;_{name}_**のように指定します。Dearは静的テキストで、 nameはフォームデータモデル要素名です。 実行時に、これはname要素の値に応じて&#x200B;**Dear Gloria Rios**または&#x200B;**Dear John Jacobs**に解決されます。

リッチテキストエディターは、ビジネスユーザーがテキストを作成し、フォームデータ要素を挿入するのに十分な直感的な機能です。 ドキュメントフラグメントエディターでは、テキストの書式設定、フォントの種類とスタイルの指定、特殊文字の挿入、ハイパーリンクの作成を行うことができます。

また、ドキュメントフラグメントエディターでは、この[ビデオ](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)で示すように、テキストにインライン条件を挿入する機能も備えています。

>[!NOTE]
>
>ドキュメントフラグメントに挿入するフォームデータモデル要素が、ルート要素の子孫であることを確認します。 例えば、この使用例では、選択したUserオブジェクトの要素がbalancesオブジェクトの子であることを確認します

