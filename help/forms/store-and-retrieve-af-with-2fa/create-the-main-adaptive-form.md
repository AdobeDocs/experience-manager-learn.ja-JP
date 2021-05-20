---
title: メインのアダプティブフォームを作成する
description: 申込者の情報とアダプティブフォームを取り込むためのアダプティブフォームを作成し、保存されたアダプティブフォームを取得する
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6552
thumbnail: 6552.jpg
topic: 開発
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 1%

---


# メインのアダプティブフォームを作成する

フォーム&#x200B;**StoreAFWithAttachments**&#x200B;は、メインのアダプティブフォームです。 このアダプティブフォームは、ユースケースのエントリポイントです。 このフォームでは、モバイル番号を含むユーザーの詳細が取り込まれます。 このフォームには、添付ファイルを追加する機能もあります。 「保存して終了」ボタンがクリックされると、サーバー側コードが実行され、フォームデータがデータベースに保存され、一意のアプリケーションIDが生成され、ユーザーに表示されて安全に保持されます。 このアプリケーションIDは、アプリケーションに関連付けられたモバイル番号を取得するために使用されます。

![主申請書](assets/6552.JPG)

このフォームは、コースで前に作成した&#x200B;**bootboxjs540,storeAFWithAttachments**&#x200B;クライアントライブラリと、フォーム送信時にトリガーされるAEMワークフローに関連付けられています。


* サンプルフォームは、サンプルフォームが正しくレンダリングされるためにAEMに読み込む必要がある、[カスタムのアダプティブフォームテンプレート](assets/custom-template-with-page-component.zip)に基づいています。

* 完成した[StoreAfWithAttachments Form](assets/store-af-with-attachments-form.zip)をダウンロードして、AEMインスタンスに読み込むことができます。

* フォームを機能させるには、このフォームに関連付けられたAEMワークフロー[をAEMインスタンスに読み込む必要があります。](assets/workflow-model-store-af-with-attachments.zip)



