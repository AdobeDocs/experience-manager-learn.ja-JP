---
title: IC ドキュメント用フォームデータモデルの作成
description: AEM Formsでフォームデータモデルを作成して、インタラクティブ通信ドキュメントのデータを動的に取得する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Interactive Communication
role: Developer
level: Intermediate
doc-type: Feature Video
duration: 170
last-substantial-update: 2026-02-20T00:00:00Z
jira: KT-20353
source-git-commit: c2dde214df0dabe8d856751a9d16afb1423e7450
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---


# IC ドキュメント用フォームデータモデルの作成

Forms AEMで外部データソースとインタラクティブ通信を統合するAdobe データモデルを作成します。 このプロセスには、RESTful サービスの設定、Swagger ファイルのアップロード、データを動的に取得してバインドするためのサービスエンドポイントの設定が含まれます。 外部サービスに安全に接続し、モデルをテストして、データ取得が成功したことを確認する方法を説明します。

開発およびテストのために注文サービスをシミュレートするモック API サーバーを実装しました。 これは、エンドポイントを公開し、特定のユーザーの注文を取得し（ユーザー ID などで）、事前に定義された、または動的に生成された注文データを実稼動 API と同じスキーマで返します。

フォームデータモデルの作成に使用する Swagger ファイルは、[ こちらからダウンロード ](assets/UsersAndOrders.json) できます。

>[!VIDEO](https://video.tv.adobe.com/v/3480005/?learn=on&enablevpops)

## 次の手順

[テンプレートの作成](./create-template.md)
