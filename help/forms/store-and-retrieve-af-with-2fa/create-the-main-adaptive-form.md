---
title: メインのアダプティブフォームの作成
description: 申込者の情報とアダプティブフォームを取り込み、保存されたアダプティブフォームを取得するためのアダプティブフォームの作成
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---


# メインのアダプティブフォームの作成

フォームStoreAFWithAttachments **** は、メインのアダプティブフォームです。 このアダプティブフォームは、使用事例へのエントリポイントです。 このフォームでは、モバイル番号を含むユーザーの詳細が取り込まれます。 このフォームには、添付ファイルを追加する機能もあります。 「Save and Exit」ボタンをクリックすると、サーバー側コードが実行され、フォームデータがデータベースに格納され、一意のアプリケーションIDが生成され、安全に保持できるようにユーザーに表示されます。 このアプリケーションIDは、アプリケーションに関連付けられているモバイル番号を取得するために使用されます。

![主申請書](assets/6552.JPG)

このフォームは、コースの前半に作成した **bootboxjs540,storeAFWithAttachments** クライアントライブラリと、フォームの送信時にトリガーされるAEMワークフローに関連付けられています。


* サンプルフォームは、 [カスタムのアダプティブフォームテンプレートに基づいており](assets/custom-template-with-page-component.zip) 、サンプルフォームが正しくレンダリングされるためにAEMに読み込む必要があります。

* 完成したStoreAfWithAttachments [フォームをダウンロードし](assets/store-af-with-attachments-form.zip) 、AEMインスタンスにインポートできます。

* フォームを機能させるには、このフォームに関連付けられた [AEMワークフローをAEMインスタンスに読み込む必要があります](assets/workflow-model-store-af-with-attachments.zip) 。



