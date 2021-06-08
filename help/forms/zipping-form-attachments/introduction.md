---
title: アダプティブフォームの添付ファイルの送信
description: アダプティブフォームの添付ファイルをzip形式で圧縮し、電子メール送信コンポーネントを使用して送信する
sub-product: フォーム[ふぉーむ]
feature: ワークフロー
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: 22437e93cbf8f36d723dc573fa327562cb51b562
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# はじめに


一般的な使用例は、アダプティブフォームの添付ファイルをzip形式で圧縮し、AEMワークフローで電子メールを送信コンポーネントを使用して送信する場合です。 この使用例を達成するために、カスタムワークフロープロセスの手順が記述されました。 このカスタムプロセスステップでは、zipファイルが作成され、フォームの添付ファイルがzipファイルに追加されます。 作成したzipファイルは、 zippedfileという名前のワークフロー変数に保存されます。

