---
title: HTM5フォーム送信時のトリガーAEMワークフロー
seo-title: HTML5フォーム送信時のトリガーAEMワークフロー
description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
seo-description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
feature: 'モバイルフォーム '
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 1%

---


# 部分的に完了したモバイルフォームをダウンロードしてAEMワークフローに送信する

一般的な使用例は、データ取得アクティビティ用にXDPをHTMLとしてレンダリングする機能を持つことです。 これは、フォームがシンプルで、オンラインで入力および送信できる場合に適しています。 ただし、フォームが複雑な場合は、ユーザーがオンラインでフォームに入力できない可能性があります。フォーム入力者がインタラクティブ版のフォームをダウンロードし、Acrobat/Readerを使用してオフラインで入力できるようにする必要があります。 フォームに入力したら、ユーザーはオンラインでフォームを送信できます。
この使用例を達成するには、次の手順を実行する必要があります。

* モバイルフォームに入力されたデータを使用してインタラクティブ/入力可能なPDFを生成する機能
* Acrobat/ReaderからのPDF送信の処理
* 送信されたPDFを確認するAdobe Experience Manager(AEM)ワークフロー

このチュートリアルでは、上記の使用例を実現するために必要な手順について説明します。 このチュートリアルに関連するサンプルコードとアセットは、[こちらから入手できます。](part-four.md)

次のビデオでは、使用例の概要を説明します

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

