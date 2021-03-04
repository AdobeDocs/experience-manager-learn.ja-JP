---
title: Webチャネル用の対話型通信の作成
seo-title: Webチャネル用の対話型通信の作成
description: これは、最初の対話型通信ドキュメントを作成するための複数手順のチュートリアルの6部目です。 ここでは、Webチャネル用のInteractive Communicationを作成します。
seo-description: これは、最初の対話型通信ドキュメントを作成するための複数手順のチュートリアルの6部目です。 ここでは、Webチャネル用のInteractive Communicationを作成します。
uuid: a1b29c5b-a323-4bda-aa99-5fb98614b690
discoiquuid: b44ff855-9ead-471e-8f0f-b562b88a5337
feature: インタラクティブコミュニケーション
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 9%

---


# Webチャネル用の対話型通信の作成

ここでは、Webチャネル用のInteractive Communicationを作成します。

1. AEM オーサーインスタンスにログインし、Adobe Experience Manager／フォーム／フォームとドキュメントに移動します。
1. 401KStatmentフォルダーを開きます。
1. 「作成」をタップし、「インタラクティブコミュニケーション」を選択します。 [対話型通信の作成]ページが表示されます。
1. 次の情報を入力します

   1. タイトル：401KStatement
   1. 説明：401KStatement（個々の参加者用）
   1. フォームデータモデル：RetiarementAccountStatement
   1. 事前入力サービス：Form Data Model Prefillサービス

1. 「次へ」をタップします
1. 次を指定します。

   1. [印刷チャネル]チェックボックスをオフにします。 印刷チャネル用のドキュメントを作成していません。
   1. Web:Webチャネルのドキュメントを生成する場合は、このオプションを選択します
   1. 対話型通信：テンプレート：**global>RetiarmentAccountStatemen** t（これは前の手順で作成したテンプレートです）
   1. テーマ：**参照テーマ —>キャンバス2.0**

1. 「作成」をタップします
1. 「完了」または「編集」をクリックして、ダイアログボックスを閉じることができます。

