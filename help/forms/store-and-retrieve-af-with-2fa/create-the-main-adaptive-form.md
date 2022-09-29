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
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 0%

---

# メインのアダプティブフォームを作成する

フォーム **StoreAFWithAttachments** はメインのアダプティブフォームです。 このアダプティブフォームは、ユースケースのエントリポイントです。 このフォームでは、モバイル番号を含むユーザーの詳細が取り込まれます。 このフォームには、添付ファイルを追加する機能もあります。 「保存して終了」ボタンがクリックされると、サーバー側コードが実行され、フォームデータがデータベースに保存され、一意のアプリケーション ID が生成され、ユーザーに表示されて安全に保持されます。 このアプリケーション ID は、アプリケーションに関連付けられているモバイル番号を取得するために使用されます。

![メインの申し込みフォーム](assets/6552.JPG)

このフォームは **bootboxjs540,storeAFWithAttachments** コースで先ほど作成したクライアントライブラリと、フォームの送信時にトリガーされるAEMワークフロー。


* サンプルフォームは、 [カスタムアダプティブフォームテンプレート](assets/custom-template-with-page-component.zip) サンプルフォームを正しくレンダリングするには、AEMに読み込む必要があります。

* 完了 [StoreAfWithAttachments フォーム](assets/store-af-with-attachments-form.zip) をダウンロードして、AEMインスタンスに読み込むことができます。

* この [このフォームに関連付けられたAEMワークフロー](assets/workflow-model-store-af-with-attachments.zip) フォームを機能させるには、AEMインスタンスに読み込む必要があります。
