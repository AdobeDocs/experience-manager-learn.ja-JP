---
title: アセット計算の拡張機能用のAdobeプロジェクトファイアルの設定
description: アセット計算プロジェクトは、特別に定義されたAdobeプロジェクトFireflyプロジェクトです。そのため、Adobe開発者コンソールでAdobeプロジェクトFireflyにアクセスして設定およびデプロイする必要があります。
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

アセット計算プロジェクトは、特別に定義されたAdobeプロジェクトFireflyプロジェクトです。そのため、Adobe開発者コンソールでAdobeプロジェクトFireflyにアクセスして設定およびデプロイする必要があります。

## AdobeデベロッパーコンソールでAdobeプロジェクトファイアフリを作成および設定します{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Adobeプロジェクトのファイアフリを設定するクリックスルー（オーディオなし）_

1. プロビジョニングされたア [カウントとサービスに関連付けられたAdobe IDを使用して、](https://console.adobe.io) Adobeデベロッパーコンソールにログインします [](./accounts-and-services.md)。 正しいAdobe組織の __システム管理者__ 、または __開発者ロール__ (Developer Role)であることを確認します。
1. Fireflyプロジェクトを作成するには、新規プロジェクトを __作成/テンプレートからプロジェクト/Project Fireflyをタップします__

   _「__&#x200B;新しいプロジェクトを&#x200B;__作成__」ボタンまたは「__プロジェクトのFirefly[」タイプが使用できない場合、Adobe組織はProject Fireflyで](#request-adobe-project-firefly)プロビジョニングされません。_

   + __プロジェクトタイトル__: `WKND AEM Asset Compute`
   + __アプリ名__: `wkndAemAssetCompute<YourName>`
      + ア __プリ名は__ 、すべてのFireflyプロジェクトで一意である必要があり、後で変更することはできません。 会社または組織の名前の前に接頭辞を付け、意味のある接尾辞を付けるのは、次のような適切な方法です。 `wkndAemAssetCompute`.
      + 自己有効化のためには、他のProject Fireflyプロジェクトとの競合を避けるなど、 __アプリ名を__&#x200B;アプリ名に接尾するのが最適です `wkndAemAssetComputeJaneDoe` 。
   + 「 __ワークスペース__ 」の下に、「 `Development`
   + 「 __Adobe I/O Runtime____」で、「各ワークスペースにランタイムを含める__ 」が選択されていることを確認します。
   + Tap __Save__ to save the project
1. AdobeFireflyプロジェクトで、ワークスペースセレクタ `Development` ーからを選択します
1. __+追加サービス/API__ をタップしてAPI ____ ウィザード追加を開きます。この方法を使用して次のAPIを追加します。

   + __Experience Cloud/アセット計算__
      + 「 __キーペアを__ 生成」を選択し __、「キーペアを__ 生成 `config.zip` 」ボタンをタップし、ダウンロードしたキーペアを安全な場所に保存して [後で使用します。](#private-key)
      + Tap __Next__
      + 製品プロファイル __統合 —Cloud Service__ を選択し、「設定済みAPIの __保存」をタップします__
   + __Adobeサービス/I/Oイベント__ /「設定済みAPIの __保存」をタップします__
   + __Adobeサービス/I/O Management API__ 、「設定済みAPIの __保存」をタップします__

## private.keyへのアクセス{#private-key}

Asset Compute APIの統合を設定する際 [、新しいキーペアが生成され、](#set-up)`config.zip` ファイルが自動的にダウンロードされました。 これ `config.zip` には、生成された公開証明書と一致する `private.key` ファイルが含まれます。

1. ファイルシステム `config.zip` 上の安全な場所に解凍します。は後で `private.key`[使用されるので、](../develop/environment-variables.md)
   + セキュリティ上、秘密鍵と秘密鍵をGitに追加しないでください。

## サービスアカウント(JWT)秘密鍵証明書の確認

このAdobeI/Oプロジェクトの資格情報は、ローカルの [Asset Compute Development Tool](../develop/development-tool.md) (AAD)でAdobe I/O Runtimeとのやり取りに使用され、Asset Computeプロジェクトに組み込む必要があります。 サービスアカウント(JWT)の資格情報を確認します。

![Adobe開発者サービスアカウント資格情報](./assets/firefly/service-account.png)

1. AdobeI/OプロジェクトFireflyプロジェクトで、ワークスペースが選択されていることを確認し `Development` ます。
1. 「 __資格情報」の下の「__ サービスアカウント(JWT) __」をタップします__
1. 表示されたAdobeI/O認証情報の確認
   + 下部に表示される __公開鍵は__ 、 __Asset Compute API__ ( `config.zip` Asset Compute API __)がこのプロジェクトに追加されたときに__ ダウンロードされたprivate.keyに対応するものです。
      + 秘密鍵が失われたり侵害されたりした場合、一致する公開鍵を削除でき、このインターフェイスを使用してAdobeI/Oで生成またはアップロードされた新しい鍵ペアを削除できます。
