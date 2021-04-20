---
title: HTM5フォーム送信時のトリガーAEMワークフロー
seo-title: HTML5フォーム送信時のトリガーAEMワークフロー
description: オフラインモードでのモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
seo-description: オフラインモードでのモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 1%

---


# 部分的に完成したモバイルフォームをダウンロードし、AEMワークフローに送信する

一般的な使用例は、データ取得アクティビティ用にXDPをHTMLとしてレンダリングする機能を持つことです。 これは、フォームがシンプルで、オンラインで入力および送信できる場合に適しています。 ただし、フォームが複雑で、オンラインでフォームを完了できない場合は、フォームの入力者がインタラクティブ版のフォームをダウンロードして、オフラインでAcrobat/Readerを使用して入力できる機能を提供する必要があります。 フォームに入力した後は、オンラインでフォームを送信できます。
この使用例を達成するには、次の手順を実行する必要があります。

* モバイルフォームに入力したデータを含むインタラクティブ/入力可能なPDFを生成する機能
* Acrobat/ReaderからのPDF送信の処理
* トリガーAdobe Experience Manager(AEM)ワークフローを使用した送信済みPDFのレビュー

このチュートリアルでは、上記の使用例を達成するために必要な手順について説明します。 このチュートリアルに関連するサンプルコードとアセットは、[こちらから参照できます。](part-four.md)

次のビデオでは、使用事例の概要を説明します

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)

