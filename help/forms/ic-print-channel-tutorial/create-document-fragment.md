---
title: ドキュメントフラグメントを作成中
description: これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの 5 部です。 ここでは、受信者の名前とアドレスを格納するドキュメントフラグメントを作成します。
seo-description: This is part 5 of a multi-step tutorial for creating your first interactive communications document. In this part, we will create document fragment to hold the recipient name and address.
uuid: 7fd8a0f2-a921-4e70-91c9-908dae9aeab2
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
kt: 5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# ドキュメントフラグメントを作成中

ここでは、受信者の名前とアドレスを格納するドキュメントフラグメントを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

ドキュメントフラグメントには、インタラクティブ通信ドキュメントのテキストコンテンツが格納されます。 このテキストコンテンツは、静的テキストにすることも、基になるデータモデル要素の値から挿入することもできます。 例： **親愛なる _{name}_**（ここで、Dear は静的テキスト、name はフォームデータモデル要素名です）。 実行時に、これはに解決されます。**Gloria Rios 様**または&#x200B;**John Jacobs 様**name 要素の値に応じて。

リッチテキストエディターは、ビジネスユーザーがテキストを作成し、フォームデータ要素を挿入するのに十分な直感的な機能です。 ドキュメントフラグメントエディターでは、テキストの書式設定、フォントの種類とスタイルの指定、特殊文字の挿入、ハイパーリンクの作成を行うことができます。

ドキュメントフラグメントエディターでは、テキストにインライン条件を挿入することもできます。 [ビデオ](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)

>[!NOTE]
>
>ドキュメントフラグメントに挿入するフォームデータモデル要素が、ルート要素の下位要素であることを確認します。 たとえば、この使用例では、選択した User オブジェクトの要素が balances オブジェクトの子であることを確認します
