---
title: アダプティブフォームの添付ファイルの送信
description: メール送信コンポーネントを使用してアダプティブフォームの添付ファイルを送信する
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-8049
exl-id: bd9e1fc1-2fc7-452c-9a4a-2e16f6821760
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '136'
ht-degree: 100%

---

# はじめに



一般的なユースケースとしては、AEM ワークフローでメール送信コンポーネントを使用してアダプティブフォームの添付ファイルを送信することが挙げられます。
通常、顧客はフォームの添付ファイルを zip 形式で圧縮するか、メール送信コンポーネントを使用して、添付ファイルを個々のファイルとして送信します。

## フォームの添付ファイルを zip ファイルで送信する

ユースケースを実現するために、カスタムワークフロープロセスの手順を作成しました。このカスタムプロセスの手順では、フォームの添付ファイルの zip ファイルを作成し、*zipped_attachments.zip* という名前のファイルのペイロードフォルダーに保存します。

![send-form-attachments](assets/send-form-attachments.JPG)

## フォームの添付ファイルを個別に送信する

ユースケースを実現するために、カスタムワークフロープロセスの手順を作成しました。このカスタムプロセスの手順では、ドキュメントの ArrayList タイプと文字列の ArrayList タイプのワークフロー変数を入力します。

![send-list-of-documents](assets/send-list-of-documents.JPG)

## 次の手順

[Zip フォームの添付ファイル](./custom-process-step.md)
