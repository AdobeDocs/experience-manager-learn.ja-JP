---
title: Asset Compute の拡張機能のために App Builder を設定する
description: Asset Compute プロジェクトは特別に定義された App Builder プロジェクトなので、Adobe Developer Console で App Builder にアクセスして設定およびデプロイする必要があります。
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '538'
ht-degree: 100%

---

# App Builder を設定する

Asset Compute プロジェクトは特別に定義された App Builder プロジェクトなので、Adobe Developer Console で App Builder にアクセスして設定およびデプロイする必要があります。

## Adobe Developer Console で App Builder を作成して設定する{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_App Builder 設定のクリックスルー（オーディオなし）_

1. プロビジョニングされた[アカウントとサービス](./accounts-and-services.md)に関連付けられた Adobe ID を使用して、[Adobe Developer Console](https://console.adobe.io) にログインします。__システム管理者__、または適切なアドビ組織の&#x200B;__デベロッパーの役割__&#x200B;であることを確認してください。
1. __プロジェクトを新規作成／テンプレートからプロジェクト／App Builder__ をタップして App Builder プロジェクトを作成します

   _「__&#x200B;プロジェクトを新規作成&#x200B;__」ボタンまたは__ App Builder __タイプのいずれかが利用できない場合、Adobe 組織が [App Builder でプロビジョニングされていません](#request-adobe-project-app-builder)。_

   + __プロジェクトタイトル__：`WKND AEM Asset Compute`
   + __アプリケーション名__：`wkndAemAssetCompute<YourName>`
      + __アプリ名__ は、すべての FApp Builderirefly プロジェクトで一意である必要があり、後で変更することはできません。 会社または組織の名前を前に付け、意味のある接尾辞を後ろに付けるのは優れたアプローチです（例：`wkndAemAssetCompute`）。
      + 自己有効化の場合、他の App Builder プロジェクトとの衝突を避けるために、`wkndAemAssetComputeJaneDoe` のように __アプリ名__&#x200B;の後に自分の名前を付けるのが最善の方法です。
   + 「__ワークスペース__」の下で、`Development` という名前の新しい環境を追加します。
   + 「__Adobe I/O Runtime__」で、「__各ワークスペースに Runtime を含める__」が選択されていることを確認してください
   + 「__保存__」をタップして、プロジェクトを保存します
1. App Builder プロジェクトで、ワークスペースセレクターから「`Development`」を選択します
1. __+ サービスの追加／API__ をタップして、__API の追加__&#x200B;ウィザードを開きます。この方法を使用して、次の API を追加します。

   + __Experience Cloud／Asset Compute__
      + 「__キーペアの生成__」を選択し、「__キーペアの生成__」ボタンをタップし、ダウンロードした `config.zip` を[後で使用するために](#private-key)安全な場所に保存します
      + 「__次へ__」をタップします。
      + 製品プロファイル&#x200B;__統合 - クラウドサービス__&#x200B;を選択して、「__設定済み API を保存__」をタップします。
   + __Adobe サービス／I/O イベント__&#x200B;と「__設定済み API を保存__」をタップします。
   + __Adobe サービス／I/O Management API__ と「__設定済み API を保存__」をタップします

## private.key へのアクセス{#private-key}

[Asset Compute API の統合](#set-up)を設定する際に、新しいキーペアが生成され、`config.zip` ファイルが自動的にダウンロードされました。この `config.zip` には、生成された公開証明書と一致する `private.key` ファイルが含まれます。

1. `private.key` は [後で使用](../develop/environment-variables.md)するため、`config.zip` をファイルシステムの安全な場所に解凍します。
   + 秘密鍵と秘密鍵は、セキュリティ上の問題として Git に追加しないでください。

## サービスアカウント（JWT）資格情報の確認

この Adobe I/O プロジェクトの資格情報は、ローカルの [Asset Compute Development Tool](../develop/development-tool.md) によって Adobe I/O ランタイムとやり取りするために使用され、Asset Compute プロジェクトに組み込む必要があります。サービスアカウント（JWT）資格情報を確認します。

![Adobe Developer サービスアカウント資格情報](./assets/app-builder/service-account.png)

1. Adobe I/O プロジェクトの App Builder プロジェクトで、`Development` ワークスペースが選択されているようにします
1. __資格情報__&#x200B;の下の「__サービスアカウント（JWT）__」をタップします
1. 表示された Adobe I/O 資格情報を確認します
   + 下部にリストされている __public key__ には、__Asset Compute API__ がこのプロジェクトに追加されたときにダウンロードされた `config.zip` の __private.key__ の対応物があります。 
      + 秘密鍵が失われたり侵害されたりした場合は、一致する公開鍵を削除でき、このインターフェイスを使用してAdobe I/O で生成またはアップロードされる新しい鍵のペアを削除できます。
