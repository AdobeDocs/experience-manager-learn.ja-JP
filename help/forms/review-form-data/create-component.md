---
title: フォームデータを一覧表示するコンポーネントを作成する
description: 送信前にフォームデータを確認するための概要コンポーネントの作成に関するチュートリアルです。
feature: Adaptive Forms
topics: development
doc-type: tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
source-git-commit: d3531e76d3341e0964e5ed878fc72037024a11fd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# フォームデータを要約するコンポーネントを作成する

レビュー用のフォームデータをリストするための単純なコンポーネントが作成されました。 この [guidebridge API の訪問関数](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit) を使用して、フォームフィールドを繰り返し処理します。 このコンポーネントに関連付けられたクライアントライブラリ内のコードは、フォーム上のパネル/テーブルコンポーネントを取得します。 このパネル/テーブルコンポーネントの子要素から、フィールドのタイトル、値、SOM 式は、GuidBridge API メソッドを使用して抽出されます。 次に、単純なHTMLテーブルは、フォームを送信する前にエンドユーザーがフォームデータを確認または編集するためのタイトル、値、SOM 式で構成されます。

例えば、以下のスクリーンショットには、フィールドとその値をリストするために作成したテーブルが表示されます。 **詳細**. TR の最後の TD は、フィールド SOM 式を使用してフィールドの値を編集するために使用されます。

![visit-func](assets/visit-function.png)

