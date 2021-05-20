---
title: Webチャネル用のインタラクティブ通信の作成
seo-title: Webチャネル用のインタラクティブ通信の作成
description: これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの第6部です。 ここでは、Webチャネル用のインタラクティブ通信を作成します。
seo-description: これは、最初のインタラクティブ通信ドキュメントを作成するための複数手順のチュートリアルの第6部です。 ここでは、Webチャネル用のインタラクティブ通信を作成します。
uuid: a1b29c5b-a323-4bda-aa99-5fb98614b690
discoiquuid: b44ff855-9ead-471e-8f0f-b562b88a5337
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 9%

---


# Webチャネル用のインタラクティブ通信の作成

ここでは、Webチャネル用のインタラクティブ通信を作成します。

1. AEM オーサーインスタンスにログインし、Adobe Experience Manager／フォーム／フォームとドキュメントに移動します。
1. 401KStatmentフォルダを開きます。
1. 「作成」をタップし、「インタラクティブ通信」を選択します。 インタラクティブ通信の作成ページが表示されます。
1. 次の情報を入力します

   1. タイトル：401KStatement
   1. 説明：個々の参加者に対する401KStatement
   1. フォームデータモデル：RetierAccountStatement
   1. 事前入力サービス：フォームデータモデルの事前入力サービス

1. 「次へ」をタップします
1. 次を指定します。

   1. 「印刷チャネル」チェックボックスをオフにします。 印刷チャネル用のドキュメントを作成していません。
   1. Web:Webチャネル用のドキュメントを生成する場合は、このオプションを選択します
   1. インタラクティブ通信：テンプレート：**global>RetiationAccountStatemen** t（前の手順で作成したテンプレート）
   1. テーマ：**参照テーマ —>Canvas 2.0**

1. 「作成」をタップします。
1. 「完了」または「編集」をクリックして、ダイアログボックスを閉じます。

