---
title: asset computeの拡張機能のための App Builder の設定
description: asset computeプロジェクトは、特別に定義された App Builder プロジェクトなので、Adobe開発者コンソールで App Builder にアクセスして設定およびデプロイする必要があります。
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# App Builder のセットアップ

asset computeプロジェクトは、特別に定義された App Builder プロジェクトなので、Adobe開発者コンソールで App Builder にアクセスして設定およびデプロイする必要があります。

## 開発者コンソールでの App Builder の作成とAdobe{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_アプリビルダー設定のクリックスルー（オーディオなし）_

1. にログインします。 [Adobe開発者コンソール](https://console.adobe.io) プロビジョニング済みのAdobe IDを使用 [アカウントとサービス](./accounts-and-services.md). 以下を実行する前に、 __システム管理者__ または __開発者ロール__ 正しいAdobeOrg
1. 次をタップして App Builder プロジェクトを作成 __新規プロジェクトを作成/テンプレートからプロジェクト/ App Builder__

   _次のいずれかの場合__&#x200B;新規プロジェクトを作成&#x200B;__ボタンまたは__ App Builder __タイプが使用できません。これは、AdobeOrg が使用できないことを意味します [App Builder でプロビジョニングされる](#request-adobe-project-app-builder)._

   + __プロジェクトタイトル__: `WKND AEM Asset Compute`
   + __アプリ名__: `wkndAemAssetCompute<YourName>`
      + この __アプリ名__ は、すべての FApp Builder Refly プロジェクトで一意である必要があり、後で変更することはできません。 会社または組織の名前の前に、意味のあるサフィックスを付けたポストフィックスは、次のような適切な方法です。 `wkndAemAssetCompute`.
      + 自己イネーブルメントの場合は、多くの場合、 __アプリ名__&#x200B;例： `wkndAemAssetComputeJaneDoe` 他の App Builder プロジェクトとの衝突を回避するため。
   + の下 __Workspaces__ 次の名前の新しい環境を追加します。 `Development`
   + の下 __Adobe I/O Runtime__ 確実にする __各ワークスペースに Runtime を含める__ が選択されています
   + タップ __保存__ プロジェクトを保存するには
1. App Builder プロジェクトで、 `Development` ワークスペースセレクターから
1. タップ __+サービスを追加/ API__ 開く __API を追加__ ウィザードでは、この方法を使用して次の API を追加します。

   + __Experience Cloud/Asset compute__
      + 選択 __キーペアを生成__ そして、 __キーペアを生成__ ボタンをクリックし、ダウンロードした `config.zip` 安全な場所に [後で使用](#private-key)
      + タップ __次へ__
      + 製品プロファイルを選択 __統合 —Cloud Service__ とタップします。 __設定済み API を保存__
   + __Adobe サービス/I/O イベント__ とタップします。 __設定済み API を保存__
   + __Adobe サービス/I/O 管理 API__ とタップします。 __設定済み API を保存__

## private.key へのアクセス{#private-key}

を設定する際に、 [asset computeAPI の統合](#set-up) 新しいキーペアが生成され、 `config.zip` ファイルが自動的にダウンロードされました。 この `config.zip` には、生成された公開証明書と照合が含まれます `private.key` ファイル。

1. 解凍 `config.zip` をファイルシステム上の `private.key` が [後で使用](../develop/environment-variables.md)
   + 秘密鍵と秘密鍵は、セキュリティ上の問題として Git に追加しないでください。

## サービスアカウント (JWT) 資格情報の確認

このAdobe I/Oプロジェクトの資格情報は、ローカルで使用されます [asset compute開発ツール](../develop/development-tool.md) Adobe I/O Runtimeとやり取りする場合、およびをAsset computeプロジェクトに組み込む必要があります。 サービスアカウント (JWT) 資格情報を確認します。

![Adobe開発者サービスアカウント資格情報](./assets/app-builder/service-account.png)

1. Adobe I/Oプロジェクトの App Builder プロジェクトで、 `Development` ワークスペースが選択されています
1. タップ __サービスアカウント (JWT)__ under __資格情報__
1. 表示されたAdobe I/O資格情報を確認します
   + この __公開鍵__ 一番下に表示されている __private.key__ ～で対応する `config.zip` ダウンロードされたタイミング __asset computeAPI__ がこのプロジェクトに追加されました。
      + 秘密鍵が失われたり侵害されたりした場合は、一致する公開鍵を削除でき、このインターフェイスを使用してAdobe I/Oで生成またはアップロードされる新しい鍵のペアを削除できます。
