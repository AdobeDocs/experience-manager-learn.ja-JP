---
title: ドキュメントフラグメントの作成
description: これは、最初のインタラクティブな通信ドキュメントを作成するためのマルチステップのチュートリアルの 第 5 部です。   ここでは、受信者の名前とアドレスを格納するドキュメントフラグメントを作成します。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 47d3aa97-0bff-48e0-8a65-55e5332f811b
jira: KT-5958
thumbnail: 22350.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 2fe3f950-bc2a-4e91-8d91-00438691727a
duration: 217
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '224'
ht-degree: 100%

---

# ドキュメントフラグメントの作成

ここでは、受信者の名前とアドレスを格納するドキュメントフラグメントを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/22350?quality=12&learn=on)

ドキュメントフラグメントには、インタラクティブ通信ドキュメントのテキストコンテンツが格納されます。 このテキストコンテンツは、静的テキストにすることも、基になるデータモデル要素の値から挿入することもできます。 例： **_{name}_**様（「様」は静的テキスト、name はフォームデータモデル要素の名前です）これは name 要素の値に応じて、実行時に&#x200B;**山田 太郎 様**や&#x200B;**鈴木 花子 様**のように解決されます。

リッチテキストエディターは、ビジネスユーザーがテキストを作成し、フォームデータ要素を挿入するのに最適な直感的な機能です。 ドキュメントフラグメントエディターでは、テキストの書式設定、フォントの種類とスタイルの指定、特殊文字の挿入、ハイパーリンクの作成を行うことができます。

この[ビデオ](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)で説明されているように、ドキュメントフラグメントエディターではテキストにインライン条件を挿入することもできます。

>[!NOTE]
>
>ドキュメントフラグメントに挿入するフォームデータモデル要素が、ルート要素の下位要素であることを確認します。 例えばこの使用例では、選択した user オブジェクトの要素が balances オブジェクトの子になります。

## 次の手順

[印刷チャネルドキュメントの作成](./create-print-channel-document.md)
