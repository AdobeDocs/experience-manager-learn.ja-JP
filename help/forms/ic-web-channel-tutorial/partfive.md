---
title: 受信者の名前と住所を格納するドキュメントフラグメントの作成
seo-title: 受信者の名前と住所を格納するドキュメントフラグメントの作成
description: 'これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 ここでは、受信者の名前と住所を格納するドキュメントフラグメントを作成します。 '
seo-description: 'これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 ここでは、受信者の名前と住所を格納するドキュメントフラグメントを作成します。 '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# 受信者名とアドレス{#creating-document-fragments-to-hold-the-recipient-name-and-address}を格納するドキュメントフラグメントの作成

ここでは、受信者の名前と住所を格納するドキュメントフラグメントを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

ドキュメントフラグメントには、インタラクティブ通信ドキュメントのテキストコンテンツが格納されます。 このテキストコンテンツは、静的テキストにすることも、基になるデータモデル要素の値から挿入することもできます。 例えば、{name}様（Dearは静的テキスト、{name}はフォームデータ要素名） 実行時に、name要素の値に応じてDear Gloria RiosまたはDear John Jacobsに解決されます。

リッチテキストエディターは、ビジネスユーザーがテキストを作成し、フォームデータ要素を挿入するのに十分な直感的な機能です。 ドキュメントフラグメントエディターでは、テキストの書式設定、フォントの種類とスタイルの指定、特殊文字の挿入、ハイパーリンクの作成を行うことができます。

また、ドキュメントフラグメントエディターでは、この[ビデオ](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)で示すように、テキストにインライン条件を挿入することもできます

>[!NOTE]
>
>ドキュメントフラグメントに挿入するフォームデータモデル要素が、ルート要素の子孫であることを確認します。 例えば、この使用例では、選択したUserオブジェクトの要素がbalancesオブジェクトの子であることを確認します

