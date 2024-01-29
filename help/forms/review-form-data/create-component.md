---
title: フォームデータを一覧表示するコンポーネントを作成する
description: 送信前にフォームデータを確認するための概要コンポーネントの作成に関するチュートリアル。
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2023-01-22T00:00:00Z
exl-id: d537a80a-de61-4d43-bdef-f7d661c43dc8
duration: 40
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '169'
ht-degree: 100%

---

# フォームデータを要約するコンポーネントの作成

レビュー用のフォームデータをリストするためのシンプルなコンポーネントが作成されました。 この [guidebridge API の訪問関数](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html?q=visit)は、フォーム フィールドを反復処理するために使用されました。 このコンポーネントに関連付けられた clientlibrary のコードは、フォーム上のパネルコンポーネントやテーブルコンポーネントを取得します。このパネルコンポーネントやテーブルコンポーネントの子要素から、フォームフィールドのタイトル、値、SOM 式が、GuidBridge API メソッドを使用して抽出されます。 続いて、フォームを送信する前にエンドユーザーがフォームデータの確認と編集ができるように、タイトル、値および SOM 式を使用して、シンプルな HTML テーブルが作成されます。

例えば、以下のスクリーンショットには、**YourDetails** のフィールドと値をリストするために作成したテーブルが表示されます。TR の最後の TD は、フィールド SOM 式を使用してフィールドの値を編集するために使用されます。

![visit-func](assets/visit-function.png)

## 次の手順

[ローカルシステムでのソリューションのテスト](./deploy-on-your-system.md)
