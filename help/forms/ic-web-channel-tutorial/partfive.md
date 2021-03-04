---
title: 受信者名とアドレスを保持するドキュメントフラグメントの作成
seo-title: 受信者名とアドレスを保持するドキュメントフラグメントの作成
description: 'これは、最初の対話型通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 この部分では、受信者名とアドレスを保持するドキュメントフラグメントを作成します。 '
seo-description: 'これは、最初の対話型通信ドキュメントを作成するための複数手順のチュートリアルの5部です。 この部分では、受信者名とアドレスを保持するドキュメントフラグメントを作成します。 '
uuid: 689931e4-a026-4e62-9acd-552918180819
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 1%

---


# 受信者名とアドレス{#creating-document-fragments-to-hold-the-recipient-name-and-address}を保持するドキュメントフラグメントの作成

この部分では、受信者名とアドレスを保持するドキュメントフラグメントを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/22350/?quality=9&learn=on)

ドキュメントフラグメントは、対話型通信ドキュメントのテキストコンテンツを保持します。 このテキストコンテンツは、静的テキストにすることも、基になるデータモデル要素の値から挿入することもできます。 例えば、{name}様（Dearはスタティックテキスト、{name}はフォームデータ要素名）。 実行時に、これはname要素の値に応じてGloria Rios様またはJohn Jacobs様に解決されます。

リッチテキストエディターは直感的な機能で、ビジネスユーザーはテキストを作成し、フォームデータ要素を挿入できます。 ドキュメントフラグメントエディターでは、テキストの書式設定、フォントの種類とスタイルの指定、特殊文字の挿入、ハイパーリンクの作成を行うことができます。

また、ドキュメントフラグメントエディターには、この[ビデオ](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)で示すように、テキストにインライン条件を挿入する機能もあります。

>[!NOTE]
>
>ドキュメントフラグメントに挿入するForm Data Model要素が、ルート要素の子孫であることを確認します。 例えば、この使用例では、選択したユーザー・オブジェクトの要素がbalancesオブジェクトの子であることを確認します

