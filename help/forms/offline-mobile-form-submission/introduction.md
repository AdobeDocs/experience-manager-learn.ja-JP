---
title: HTM5 フォーム送信での AEM ワークフローのトリガー（概要）
description: オフラインモードでのモバイルフォームへの入力の継続とモバイルフォームの送信による AEM ワークフローのトリガー
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 88295af5-3022-4462-9194-46d8c979bc8b
last-substantial-update: 2021-04-07T00:00:00Z
duration: 351
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
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
