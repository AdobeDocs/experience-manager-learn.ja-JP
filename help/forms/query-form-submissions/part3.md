---
title: クエリインターフェイスを作成
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
source-wordcount: '130'
ht-degree: 1%

---

# クエリインターフェイスを作成

「管理者」が検索条件を入力し、特定のフォーム送信を取得できるように、シンプルなクエリインターフェイスが構築されました。 結果はシンプルな表形式で表示され、特定のフォームの送信を表示するオプションが用意されています。

![query-submissions](assets/query-submissions.png)

このインターフェイスのドロップダウンは、カスケードドロップダウンです。 ドロップダウンで使用できるオプションは、前のドロップダウンでおこなった選択内容に応じて変わります。

ドロップダウンは、RESTful データソースを使用して入力されます。

検索結果は、「SearchResults」と呼ばれるカスタムコンポーネントに表示されます。 ユーザーが「表示」ボタンをクリックすると、フォームに送信されたデータと添付ファイルが事前に入力されます。

## 次の手順

[事前入力サービスの書き込み](./part4.md)
