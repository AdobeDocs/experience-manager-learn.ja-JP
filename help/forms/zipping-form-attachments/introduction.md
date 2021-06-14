---
title: アダプティブフォームの添付ファイルの送信
description: 電子メール送信コンポーネントを使用してアダプティブフォームの添付ファイルを送信する
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
source-git-commit: 540e11c0861eacc795122328b2359c7db6378aec
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 1%

---


# はじめに



一般的な使用例は、AEMワークフローで電子メールを送信コンポーネントを使用してアダプティブフォームの添付ファイルを送信する場合です。
通常、顧客はフォームの添付ファイルをzip形式で圧縮するか、電子メール送信コンポーネントを使用して添付ファイルを個々のファイルとして送信します。

## フォームの添付ファイルをzipファイルで送信する

この使用例を達成するために、カスタムワークフロープロセスの手順が記述されました。 このカスタムプロセスステップでは、作成され、*zipped_attachments.zip*&#x200B;という名前のファイルのペイロードフォルダーの下に保存されているフォーム添付ファイルを含むzipファイルを作成します。

![send-form-attachments](assets/send-form-attachments.JPG)

## フォームの添付ファイルを個別に送信する

この使用例を実現するために、カスタムワークフロープロセスの手順が記述されました。 このカスタムプロセスステップでは、ドキュメントのArrayList型と文字列のArrayList型のワークフロー変数を設定します。

![send-list-of-documents](assets/send-list-of-documents.JPG)



