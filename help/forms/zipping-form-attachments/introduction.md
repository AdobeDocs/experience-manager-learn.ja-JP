---
title: アダプティブフォームの添付ファイルを送信する
description: 電子メール送信コンポーネントを使用してアダプティブフォームの添付ファイルを送信する
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# はじめに



一般的な使用例は、AEMワークフローで電子メールを送信コンポーネントを使用してアダプティブフォームの添付ファイルを送信する場合です。
通常、顧客はフォームの添付ファイルを zip 形式で圧縮するか、電子メールの送信コンポーネントを使用して添付ファイルを個々のファイルとして送信します。

## フォームの添付ファイルを zip ファイルで送信する

この使用例を達成するために、カスタムワークフロープロセスの手順が記述されました。 このカスタムプロセスステップでは、フォームの添付ファイルが作成され、という名前のファイルの payload フォルダーの下に保存されている zip ファイルを *zipped_attachments.zip*

![send-form-attachments](assets/send-form-attachments.JPG)

## フォームの添付ファイルを個別に送信する

この使用例を達成するために、カスタムワークフロープロセスの手順が記述されました。 このカスタムプロセスステップでは、ドキュメントの ArrayList 型のワークフロー変数と文字列の ArrayList 型のワークフロー変数を設定します。

![send-list-of-documents](assets/send-list-of-documents.JPG)
