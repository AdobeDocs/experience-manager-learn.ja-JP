---
title: asset computeの拡張機能用にAdobeプロジェクトの蛍光ファイルを設定します
description: asset computeプロジェクトとは、特別に定義されたAdobeプロジェクトFireflyプロジェクトです。そのため、Adobeプロジェクトを設定してデプロイするには、Adobe開発者コンソールでプロジェクトFireflyにアクセスする必要があります。
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Adobeプロジェクトの蛍光を設定

asset computeプロジェクトとは、特別に定義されたAdobeプロジェクトFireflyプロジェクトです。そのため、Adobeプロジェクトを設定してデプロイするには、Adobe開発者コンソールでプロジェクトFireflyにアクセスする必要があります。

## Adobe開発者コンソールでAdobeプロジェクトのFireflyを作成し、設定します{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Adobeプロジェクトのファイアフリを設定するクリックスルー（オーディオなし）_

1. プロビジョニングされた[アカウントおよびサービス](./accounts-and-services.md)に関連付けられたAdobe IDを使用して、[Adobe開発者コンソール](https://console.adobe.io)にログインします。 正しいAdobe組織の&#x200B;__システム管理者__&#x200B;または&#x200B;__開発者ロール__&#x200B;であることを確認してください。
1. __新しいプロジェクトを作成/テンプレートからプロジェクト/Project Firefly__&#x200B;をタップして、Fireflyプロジェクトを作成します。

   _「__&#x200B;新しいプロジェクトを&#x200B;__作成」ボタンまたは「__&#x200B;プロジェクトの&#x200B;__Fireflytype」が使用できない場合、Adobe組織はProject Firefly [でプロビジョニングされません](#request-adobe-project-firefly)。_

   + __プロジェクトタイトル__:  `WKND AEM Asset Compute`
   + __アプリ名__:  `wkndAemAssetCompute<YourName>`
      + __アプリ名__&#x200B;は、すべてのFireflyプロジェクトで一意である必要があり、後で変更することはできません。 会社または組織の名前の前に接頭辞を付け、意味のある接尾辞を付けるのは、次のような適切な方法です。`wkndAemAssetCompute`.
      + 自己有効化のためには、__アプリ名__&#x200B;に名前を接尾することをお勧めします。例えば、`wkndAemAssetComputeJaneDoe`は、他のProject Fireflyプロジェクトとの衝突を避けるためです。
   + __Workspaces__&#x200B;の下に、`Development`という名前の新しい環境を追加します
   + __Adobe I/O Runtime__&#x200B;の下で、「__各ワークスペース__&#x200B;にランタイムを含める」が選択されていることを確認します
   + 「__保存__」をタップして、プロジェクトを保存します
1. AdobeFireflyプロジェクトで、ワークスペースセレクターから`Development`を選択します
1. __+追加サービス/API__&#x200B;をタップして&#x200B;__API__&#x200B;ウィザードを開きます。追加この方法を使用して次のAPIを追加します。

   + __Experience Cloud/Asset compute__
      + 「__キーペアを生成__」を選択し、「__キーペアを生成__」ボタンをタップします。ダウンロードした`config.zip`を[後で使用する](#private-key)の安全な場所に保存します。
      + __次へ__&#x200B;をタップします
      + 製品プロファイル&#x200B;__Cloud Service — 統合__&#x200B;を選択し、__設定済みAPIの保存__&#x200B;をタップします
   + __Adobeサービス/I/O__ イベントを選択し、「設定済みAPIの __保存」をタップします__
   + __Adobeサービス/I/O管理__ APIを選択し、「設定済みAPIの __保存」をタップします__

## private.key{#private-key}にアクセスします

[Asset computeAPI統合](#set-up)を設定する際に、新しいキーペアが生成され、`config.zip`ファイルが自動的にダウンロードされました。 この`config.zip`には、生成された公開証明書と一致する`private.key`ファイルが含まれています。

1. `private.key`は[後で](../develop/environment-variables.md)使用されるため、`config.zip`をファイルシステム上の安全な場所に解凍します。
   + セキュリティ上、秘密鍵と秘密鍵をGitに追加しないでください。

## サービスアカウント(JWT)秘密鍵証明書の確認

このAdobe I/Oプロジェクトの資格情報は、ローカルの[Asset compute開発ツール](../develop/development-tool.md)がAdobe I/O Runtimeとのやり取りに使用し、Asset computeプロジェクトに組み込む必要があります。 サービスアカウント(JWT)の資格情報を確認します。

![Adobe開発者サービスアカウント資格情報](./assets/firefly/service-account.png)

1. Adobe I/OプロジェクトのFireflyプロジェクトで、`Development`ワークスペースが選択されていることを確認します。
1. __資格情報__&#x200B;の下の&#x200B;__サービスアカウント(JWT)__&#x200B;をタップします
1. 表示されたAdobe I/O資格情報の確認
   + 下部に表示される&#x200B;__公開鍵__&#x200B;は、__Asset computeAPI__&#x200B;がこのプロジェクトに追加されたときに、`config.zip`内の&#x200B;__private.key__&#x200B;に対応するものです。
      + 秘密鍵が失われたり侵害されたりした場合、一致する公開鍵を削除でき、このインターフェイスを使用してAdobe I/Oで生成またはアップロードされた新しい鍵ペアを削除できます。
