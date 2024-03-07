---
title: フォーム送信に対するクエリ
description: Azure ポータルに保存されたフォーム送信のクエリに関する手順を説明するマルチパートチュートリアル
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
source-git-commit: ae2a2cbde1bf21314cc77863014cb0f013b6e0bb
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 3%

---

# カスタム送信の作成

フォームの送信を処理するために、カスタム送信ハンドラーが書き込まれました。 高レベルでは、カスタム送信ハンドラーは次の処理を実行します

* 送信されたフォームの名前を抽出します。
* 送信されたデータを抽出します。コアコンポーネントベースのフォームの送信されたデータは、常に JSON 形式になります。
* フォームの添付ファイルを抽出して Azure ポータルに保存します。 送信された json データを添付ファイルの URL で更新します。
* BLOB インデックスタグを作成 — フォームの検索可能なフィールドのリストと、その対応する値を送信されたデータから検索します。
* BLOB インデックスタグを送信されたデータに関連付け、Azure ポータルに保存します。

次のスクリーンショットは、Azure ポータルの BLOB インデックスタグを示しています。

![blob-index-tags](assets/blob-index-tags.png)

カスタム送信コードは、 **_StoreFormDataWithBlobIndexTagsInAzure_** Azure からデータを保存および取得するためのコードがコンポーネントに含まれている。 **_SaveAndFetchFromAzure_**

## 次の手順

[クエリインターフェイスを作成](./part3.md)

