---
title: リポジトリブラウザでのAEMのデバッグ
description: リポジトリブラウザーは、AEMの基になるデータストアを表示し、AEMのas a Cloud Service環境を簡単にデバッグできる強力なツールです。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 10004
thumbnail: 341464.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 88af40fc-deff-4b92-84b1-88df2dbdd90b
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# リポジトリブラウザーでas a Cloud ServiceしたAEMのデバッグ

リポジトリブラウザーは、AEMの基になるデータストアを表示し、AEMのas a Cloud Service環境を簡単にデバッグできる強力なツールです。 リポジトリブラウザーは、実稼動、ステージング、開発上のAEMのリソースとプロパティ、および作成者、公開、プレビューの各サービスの読み取り専用表示をサポートします。

>[!VIDEO](https://video.tv.adobe.com/v/341464?quality=12&learn=on)

リポジトリブラウザー： __のみ__ AEMas a Cloud Service環境で使用可能 ( [CRXDE Lite](../aem-sdk-local-quickstart/other-tools.md#crxde-lite) ローカルAEM SDK のデバッグ ) を参照してください。

## リポジトリブラウザへのアクセス

AEM as a Cloud Serviceのリポジトリブラウザにアクセスするには：

1. ユーザーが [必要なアクセス](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#access-prerequisites)
1. にログインします。 [Cloud Manager](https://my.cloudmanager.adobe.com)
1. デバッグするAEM as a Cloud Service環境を含むプログラムを選択します。
1. を開きます。 [開発者コンソール](./developer-console.md) デバッグするAEMas a Cloud Service環境に対応
1. を選択します。 __リポジトリブラウザ__ タブ
1. 参照するAEMサービス層を選択
   + すべての発言者
   + すべての発行者
   + すべてのプレビュー
1. 選択 __リポジトリブラウザを開く__

リポジトリブラウザーは、選択したサービス層（オーサー、パブリッシュ、プレビュー）の読み取り専用モードで開き、ユーザーがアクセスできるリソースおよびプロパティを表示します。

## アクセスの公開とプレビュー

デフォルトでは、「公開」または「プレビュー」へのアクセスは制限され、リポジトリブラウザーで使用可能なリソースが減少します。 [「公開」（または「プレビュー」）ですべてのリソースを表示するには、ユーザーを「公開」（または「プレビュー」）管理者の役割に追加します。](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developer-tools/repository-browser.html#navigate-the-hierarchy)
