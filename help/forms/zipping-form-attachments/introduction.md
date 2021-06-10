---
title: アダプティブフォームの添付ファイルの送信
description: アダプティブフォームの添付ファイルをzip形式で圧縮し、電子メール送信コンポーネントを使用して送信する
feature: アダプティブフォーム
topics: adaptive forms
audience: developer
doc-type: article
activity: setup
version: 6.5
topic: 開発
role: Developer
level: Beginner
kt: kt-8049
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '78'
ht-degree: 2%

---


# はじめに



一般的な使用例は、アダプティブフォームの添付ファイルをzip形式で圧縮し、AEMワークフローで電子メールを送信コンポーネントを使用して送信する場合です。 この使用例を達成するために、カスタムワークフロープロセスの手順が記述されました。 このカスタムプロセスステップでは、作成され、*zipped_attachments.zip*&#x200B;という名前のファイルのペイロードフォルダーの下に保存されているフォーム添付ファイルを含むzipファイルを作成します。

![send-form-attachments](assets/send-form-attachments.JPG)


