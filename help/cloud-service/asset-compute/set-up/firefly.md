---
title: Adobeの拡張機能用のAsset computeProject Fireflyの設定
description: asset computeプロジェクトは、特別に定義されたAdobeProject Fireflyプロジェクトです。そのため、設定およびデプロイするには、Adobe開発者コンソールのAdobeProject Fireflyにアクセスする必要があります。
feature: asset computeマイクロサービス
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: 統合、開発
role: Developer
level: Intermediate, Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 0%

---


# AdobeProject Fireflyの設定

asset computeプロジェクトは、特別に定義されたAdobeProject Fireflyプロジェクトです。そのため、設定およびデプロイするには、Adobe開発者コンソールのAdobeProject Fireflyにアクセスする必要があります。

## Adobe開発者コンソールでAdobeProject Fireflyを作成し、設定します。{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Project Fireflyの設定のクリックスルー(Adobeなし)_

1. プロビジョニングされた[アカウントおよびサービスに関連付けられたAdobe IDを使用して、[Adobe開発者コンソール](https://console.adobe.io)にログインします。](./accounts-and-services.md) 正しいAdobe組織については、__システム管理者__&#x200B;または&#x200B;__デベロッパーロール__&#x200B;であることを確認します。
1. __新しいプロジェクトを作成/テンプレートからプロジェクト/ Project Firefly__&#x200B;をタップして、Fireflyプロジェクトを作成します。

   _「新しいプロジ__&#x200B;ェクトを作&#x200B;__成」ボタンまたは「__&#x200B;プロジ&#x200B;__ェクトFireflytype」が使用できない場合、Adobe組織にProject Fireflyがプロビジョニ [ングされていません](#request-adobe-project-firefly)。_

   + __プロジェクトタイトル__:  `WKND AEM Asset Compute`
   + __アプリ名__:  `wkndAemAssetCompute<YourName>`
      + __アプリ名__&#x200B;は、すべてのFireflyプロジェクトで一意である必要があり、後で変更することはできません。 会社または組織の名前の前に、意味のあるサフィックスを付けるポストフィックスは、次のような適切な方法です。`wkndAemAssetCompute`.
      + 自己イネーブルメントの場合は、他のProject Fireflyプロジェクトとの競合を避けるために、__アプリ名__（例：`wkndAemAssetComputeJaneDoe`）に名前を接尾することをお勧めします。
   + __Workspaces__&#x200B;の下に、`Development`という名前の新しい環境を追加します。
   + __Adobe I/O Runtime__&#x200B;の下で、「__Include Runtime with each workspace__」が選択されていることを確認します。
   + 「__保存__」をタップして、プロジェクトを保存します。
1. AdobeFireflyプロジェクトで、ワークスペースセレクターから`Development`を選択します。
1. __+サービスを追加/API__&#x200B;をタップして&#x200B;__APIを追加__&#x200B;ウィザードを開きます。次のAPIを追加するには、この方法を使用します。

   + __Experience Cloud/Asset compute__
      + 「__キーペアを生成__」を選択し、「__キーペアを生成__」ボタンをタップし、ダウンロードした`config.zip`を[後で](#private-key)を使用する安全な場所に保存します。
      + __次へ__&#x200B;をタップします
      + 製品プロファイル「__Integrations - integrations__」を選択し、「__設定済みAPIを保存__」をタップします。
   + __Adobeサービス/I/Oイベントを選択__ し、「設定済みAPIを __保存」をタップします。__
   + __Adobeサービス/I/O管理APIを選択__ し、「設定済みAPIを __保存」をタップします。__

## private.key{#private-key}へのアクセス

[Asset computeAPI統合](#set-up)を設定すると、新しいキーペアが生成され、`config.zip`ファイルが自動的にダウンロードされました。 この`config.zip`には、生成された公開証明書と一致する`private.key`ファイルが含まれます。

1. `private.key`は[後で](../develop/environment-variables.md)使用されるので、`config.zip`をファイルシステム上の安全な場所に解凍します。
   + 秘密鍵と秘密鍵は、セキュリティ上の問題からGitに追加しないでください。

## サービスアカウント(JWT)資格情報の確認

このAdobe I/Oプロジェクトの資格情報は、ローカルの[Asset compute開発ツール](../develop/development-tool.md)でAdobe I/O Runtimeとのやり取りに使用され、Asset computeプロジェクトに組み込む必要があります。 サービスアカウント(JWT)資格情報を確認します。

![Adobe開発者サービスアカウント資格情報](./assets/firefly/service-account.png)

1. Adobe I/OProject Fireflyプロジェクトから、`Development`ワークスペースが選択されていることを確認します。
1. __Credentials__&#x200B;の下の「__サービスアカウント(JWT)__」をタップします。
1. 表示されたAdobe I/O資格情報を確認します
   + 下部に表示される&#x200B;__公開鍵__&#x200B;は、__Asset computeAPI__&#x200B;がこのプロジェクトに追加されたときに、`config.zip`に&#x200B;__private.key__&#x200B;と対応するものです。
      + 秘密鍵が失われたり侵害されたりした場合、一致する公開鍵は削除され、このインターフェイスを使用してで生成またはAdobe I/Oにアップロードされる新しいキーペアが作成されます。
