---
title: メインのアダプティブフォームの作成
description: 申込者の情報とアダプティブフォームを取り込み、保存されたアダプティブフォームを取得するためのアダプティブフォームの作成
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: 開発
role: 開業医
level: 初心者
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 1%

---


# メインのアダプティブフォームの作成

フォーム&#x200B;**StoreAFWithAttachments**&#x200B;はメインのアダプティブフォームです。 このアダプティブフォームは、使用事例へのエントリポイントです。 このフォームでは、モバイル番号を含むユーザーの詳細が取り込まれます。 このフォームには、添付ファイルを追加する機能もあります。 「Save and Exit」ボタンをクリックすると、サーバー側コードが実行され、フォームデータがデータベースに格納され、一意のアプリケーションIDが生成され、安全に保持できるようにユーザーに表示されます。 このアプリケーションIDは、アプリケーションに関連付けられているモバイル番号を取得するために使用されます。

![主申請書](assets/6552.JPG)

このフォームは、コースの前のセクションで作成した&#x200B;**bootboxjs540,storeAFWithAttachments**&#x200B;クライアントライブラリと、フォームの送信時にトリガーされるAEMワークフローに関連付けられています。


* サンプルフォームは、サンプルフォームが正しくレンダリングされるためにAEMに読み込む必要がある、[カスタムのアダプティブフォームテンプレート](assets/custom-template-with-page-component.zip)に基づいています。

* 完成した[StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip)は、AEMインスタンスにダウンロードしてインポートできます。

* フォームを機能させるには、このフォーム](assets/workflow-model-store-af-with-attachments.zip)に関連付けられた[AEMワークフローをAEMインスタンスにインポートする必要があります。



