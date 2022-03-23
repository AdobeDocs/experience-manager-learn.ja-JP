---
title: HTM5 フォーム送信でのトリガーAEMワークフローの概要
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: モバイルフォームをオフラインモードで入力し続け、モバイルフォームをトリガーAEMワークフローに送信します
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 部分的に完了したモバイルフォームをダウンロードし、AEMワークフローに送信する

一般的な使用例は、データ取得アクティビティのHTMLとして XDP をレンダリングできるようにすることです。 これは、フォームがシンプルで、オンラインで入力および送信できる場合に適しています。 ただし、フォームが複雑な場合は、ユーザーがオンラインでフォームに入力できない可能性があります。フォーム入力者が、オフラインでAcrobat/Readerを使用して入力するインタラクティブバージョンをダウンロードできるようにする必要があります。 フォームに入力したら、ユーザーはオンラインでフォームを送信できます。
この使用例を達成するには、次の手順を実行する必要があります。

* モバイルフォームに入力したデータを使用して、インタラクティブ/入力可能なPDFを生成する機能
* Acrobat/ReaderからのPDF送信を処理
* トリガー済みのAdobe Experience Manager(AEM) ワークフローを使用して送信済みのPDFを確認

このチュートリアルでは、上記の使用例を達成するために必要な手順について説明します。 このチュートリアルに関連するサンプルコードとアセットは次のとおりです。 [こちらからご利用いただけます。](part-four.md)

次のビデオでは、使用例の概要を説明します

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=9&learn=on)
