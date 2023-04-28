---
title: HTM5 フォーム送信での AEM ワークフローのトリガー（概要）
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: オフラインモードでのモバイルフォームへの入力の継続とモバイルフォームの送信による AEM ワークフローのトリガー
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
last-substantial-update: 2021-04-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '205'
ht-degree: 100%

---

# 部分的に完了したモバイルフォームのダウンロードと AEM ワークフローへの送信

一般的なユースケースは、データ取得アクティビティで XDP を HTML としてレンダリングできるようにすることです。 これは、フォームがシンプルで、オンラインで入力および送信できる場合に適しています。 ただし、フォームが複雑な場合にユーザーがオンラインでフォームを完成できない可能性があるので、オフラインで Acrobat／Reader を使用して入力するインタラクティブバージョンをダウンロードできるようにする必要があります。フォームに入力したら、ユーザーはオンラインでフォームを送信できます。
このユースケースを達成するには、次の手順を実行する必要があります。

* モバイルフォームに入力したデータを使用して、インタラクティブ／入力可能な PDF を生成します。
* Acrobat／Reader からの PDF 送信を処理します。
* Adobe Experience Manager（AEM）ワークフローをトリガーして、送信された PDF を確認します。

このチュートリアルでは、上記のユースケースを達成するために必要な手順について説明します。 このチュートリアルに関連するサンプルコードとアセットは、[こちらから入手](part-four.md)できます。

次のビデオでは、ユースケースの概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/29677?quality=12&learn=on)
