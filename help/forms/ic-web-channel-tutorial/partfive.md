---
title: 受信者の名前とアドレスを格納するドキュメントフラグメントの作成
description: これは、最初のインタラクティブな通信ドキュメントを作成するためのマルチステップのチュートリアルの 第 5 部です。   ここでは、受信者の名前とアドレスを格納するドキュメントフラグメントを作成します。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 404eed65-ec55-492a-85b5-59773896b217
topic: Development
role: Developer
level: Beginner
exl-id: 1d7093a8-3765-46ec-912a-b5a5503fd5af
duration: 219
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '239'
ht-degree: 100%

---

# 受信者の名前とアドレスを格納するドキュメントフラグメントの作成 {#creating-document-fragments-to-hold-the-recipient-name-and-address}

ここでは、受信者の名前とアドレスを格納するドキュメントフラグメントを作成します。

>[!VIDEO](https://video.tv.adobe.com/v/36369?quality=12&learn=on&captions=jpn)

ドキュメントフラグメントには、インタラクティブ通信ドキュメントのテキストコンテンツが格納されます。 このテキストコンテンツは、静的テキストにすることも、基になるデータモデル要素の値から挿入することもできます。 例：{name} 様（「様」は静的テキスト、「{name}」はフォームデータ要素名）。 実行時に、これは name 要素の値に応じて、Gloria Rios 様または John Jacobs 様に解決されます。

リッチテキストエディターでは、ビジネスユーザーは直感的にテキストを作成し、フォームデータ要素を挿入できます。ドキュメントフラグメントエディターでは、テキストの書式設定、フォントの種類とスタイルの指定、特殊文字の挿入、ハイパーリンクの作成を行うことができます。

また、ドキュメントフラグメントエディターでは、テキストにインライン条件を挿入する機能もあります（以下の[ビデオ](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)を参照）。

>[!NOTE]
>
>ドキュメントフラグメントに挿入するフォームデータモデル要素が、ルート要素の下位要素であることを確認します。 例えばこの使用例では、選択した user オブジェクトの要素が balances オブジェクトの子になります。

## 次の手順

[インタラクティブ通信ドキュメントの作成](./partsix.md)