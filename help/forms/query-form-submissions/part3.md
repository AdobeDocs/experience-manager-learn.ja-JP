---
title: クエリインターフェイスの作成
description: Azure ポータルに保存されたフォーム送信のクエリに関係する手順を説明するマルチパートチュートリアル
feature: Adaptive Forms
doc-type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-14884
last-substantial-update: 2024-03-03T00:00:00Z
exl-id: 570a8176-ecf8-4a57-ab58-97190214c53f
duration: 25
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '130'
ht-degree: 100%

---

# クエリインターフェイスの作成

「管理者」が検索条件を入力し、特定のフォーム送信を取得できるように、シンプルなクエリインターフェイスが構築されました。結果はシンプルな表形式で表示され、特定のフォーム送信を表示するオプションが用意されています。

![query-submissions](assets/query-submissions.png)

このインターフェイスのドロップダウンは、カスケードドロップダウンです。ドロップダウンで使用できるオプションは、前のドロップダウンでの選択に基づいて変化します。

ドロップダウンは、RESTful データソースを使用して設定されます。

検索結果は、「SearchResults」と呼ばれるカスタムコンポーネントに表示されます。ユーザーが「表示」ボタンをクリックすると、フォームには送信されたデータと添付ファイルが事前に入力されます。

## 次の手順

[事前入力サービスの記述](./part4.md)
