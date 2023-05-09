---
title: メインのアダプティブフォームを作成する
description: アダプティブフォームを作成して申込者の情報を取得し、保存されたアダプティブフォームを取得する
feature: Adaptive Forms
type: Tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: Development
role: User
level: Beginner
exl-id: 73de0ac4-ada6-4b8e-90a8-33b976032135
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 97%

---

# メインのアダプティブフォームを作成する

フォーム **StoreAFWithAttachments** はメインのアダプティブフォームです。このアダプティブフォームは、ユースケースのエントリポイントです。このフォームでは、モバイル番号を含むユーザーの詳細が取り込まれます。このフォームには、添付ファイルを追加する機能もあります。「保存して終了」ボタンがクリックされると、サーバーサイドコードが実行されてフォームデータがデータベースに保存されます。一意のアプリケーション ID が生成され、ユーザーに表示されて安全に保持されます。このアプリケーション ID は、アプリケーションに関連付けられているモバイル番号を取得するために使用されます。

![メインのアプリケーションフォーム](assets/6552.JPG)

このフォームは、コースの前半で作成された **bootboxjs540,storeAFWithAttachments** クライアントライブラリと、フォームの送信時にトリガーされる AEM ワークフローに関連付けられています。


* サンプルフォームは、[カスタムアダプティブフォームテンプレート](assets/custom-template-with-page-component.zip)に基づいており、サンプルフォームが正しくレンダリングされるように AEM に読み込む必要があります。

* 完成した [StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip) をダウンロードして、AEM インスタンスに読み込みます。

* [このフォームに関連付けられた AEM ワークフロー](assets/workflow-model-store-af-with-attachments.zip)は、フォームを機能させるために AEM インスタンスに読み込む必要があります。


## 次の手順

[保存済みフォームを取得するフォームを作成する](./retrieve-saved-form.md)