---
title: AEMコンテンツフラグメントコンソール拡張機能のデプロイ
description: AEMコンテンツフラグメントコンソール拡張機能をデプロイする方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---


# 拡張機能のデプロイ

AEMas a Cloud Serviceの環境で使用するには、拡張機能 App Builder アプリケーションをデプロイし、承認する必要があります。

拡張機能の App Builder アプリをデプロイする際に注意すべき考慮事項がいくつかあります。

+ 拡張機能は、Adobe Developer Console プロジェクトワークスペースにデプロイされます。 デフォルトのワークスペースは次のとおりです。
   + __実稼動__ workspace には、すべてのAEM as a Cloud Serviceで使用できる拡張機能のデプロイメントが含まれます。
   + __ステージ__ workspace は、開発者ワークスペースとして機能します。 ステージワークスペースにデプロイされた拡張機能は、AEM as a Cloud Serviceでは使用できません。
Adobe Developerコンソールのワークスペースには、AEM as a Cloud Serviceの環境タイプとの直接の相関関係はありません。
+ 実稼動ワークスペースにデプロイされた拡張機能は、Adobe組織内の、拡張機能が存在するすべてのAEMas a Cloud Service環境に表示されます。
拡張機能は、 [AEMas a Cloud Serviceホスト名を確認する条件付きロジック](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ AEM as a Cloud Serviceでは複数の拡張機能を使用できます。 Adobeでは、単一のビジネス目標を解決するために、各拡張機能 App Builder アプリを使用することをお勧めします。 ただし、1 つの拡張機能 App Builder アプリケーションは、共通のビジネス目標をサポートする複数の拡張ポイントを実装できます。

## 初期デプロイメント

拡張機能をAEMas a Cloud Serviceの環境で使用するには、Adobe Developerコンソールにデプロイする必要があります。

デプロイメントプロセスは、次の 2 つの論理ステップに分かれます。

1. 開発者がAdobe Developer Console に拡張機能 App Builder アプリをデプロイする。
1. デプロイメントマネージャーまたはビジネスオーナーによる拡張機能の承認。

### 拡張機能の App Builder アプリをデプロイする

拡張機能を実稼動ワークスペースにデプロイします。 実稼動ワークスペースにデプロイされた拡張機能は、拡張機能のデプロイ先のAdobe組織内のすべてのAEMas a Cloud Serviceオーサーサービスに自動的に追加されます。

1. 更新された拡張機能 App Builder アプリケーションのルートへのコマンドラインを開きます。
1. 実稼動ワークスペースがアクティブであることを確認します。

   ```shell
   $ aio app use -w Production
   ```

   変更を `.env` および `.aio`.

1. 更新された拡張機能 App Builder アプリをデプロイします。

   ```shell
   $ aio app deploy
   ```

#### デプロイメントの承認をリクエスト

![承認用に拡張機能を送信](./assets/deploy/submit-for-approval.png){align="center"}

1. にログインします。 [Adobe Developer Console](https://developer.adobe.com)
1. 選択 __コンソール__
1. に移動します。 __プロジェクト__
1. 拡張機能に関連付けられたプロジェクトを選択
1. を選択します。 __実稼動__ workspace
1. 選択 __承認用に送信__
1. 必要に応じてフィールドを更新し、フォームに入力して送信します。

+ アイコンが必要です。 アイコンがない場合、 [このアイコン](./assets/deploy/icon.png).

### デプロイメントリクエストの承認

![拡張機能の承認](./assets/deploy/adobe-exchange.png){align="center"}

1. にログインします。 [Adobe交換](https://exchange.adobe.com/)
1. に移動します。 __管理__ > __レビュー保留中のアプリ__
1. __レビュー__ 拡張機能の App Builder アプリケーション
1. 拡張機能の変更が許容できる場合 __確定__ レビュー。 これにより、直ちに、Adobe組織内のすべてのAEMas a Cloud Serviceオーサーサービスに拡張機能が挿入されます。

拡張機能のリクエストが承認されると、直ちにAEMas a Cloud Serviceのオーサーサービスで拡張機能がアクティブになります。

## 拡張機能の更新

App Builder アプリを更新して拡張する場合は、 [初期デプロイメント](#initial-deployment)に値を入力し、既存の拡張機能のデプロイメントを取り消す必要があるという偏差値を指定します。

### 拡張機能を取り消す

新しいバージョンの拡張機能をデプロイするには、まずその拡張機能を取り消す（または削除する）必要があります。 拡張機能が失効している間は、AEMコンソールでは使用できません。

1. にログインします。 [Adobe交換](https://exchange.adobe.com/)
1. に移動します。 __管理__ > __App Builder アプリ__
1. __失効__ 更新する拡張機能

### 拡張機能のデプロイ

拡張機能を実稼動ワークスペースにデプロイします。 実稼動ワークスペースにデプロイされた拡張機能は、拡張機能のデプロイ先のAdobe組織内のすべてのAEMas a Cloud Serviceオーサーサービスに自動的に追加されます。

1. 更新された拡張機能 App Builder アプリケーションのルートへのコマンドラインを開きます。
1. 実稼動ワークスペースがアクティブであることを確認します。

   ```shell
   $ aio app use -w Production
   ```

   変更を `.env` および `.aio`.

1. 更新された拡張機能 App Builder アプリをデプロイします。

   ```shell
   $ aio app deploy
   ```

#### デプロイメントの承認をリクエスト

![承認用に拡張機能を送信](./assets/deploy/submit-for-approval.png){align="center"}

1. にログインします。 [Adobe Developer Console](https://developer.adobe.com)
1. 選択 __コンソール__
1. に移動します。 __プロジェクト__
1. 拡張機能に関連付けられたプロジェクトを選択
1. を選択します。 __実稼動__ workspace
1. 選択 __承認用に送信__
1. 必要に応じてフィールドを更新し、フォームに入力して送信します。

#### デプロイメントリクエストの承認

![拡張機能の承認](./assets/deploy/adobe-exchange.png){align="center"}

1. にログインします。 [Adobe交換](https://exchange.adobe.com/)
1. に移動します。 __管理__ > __レビュー保留中のアプリ__
1. __レビュー__ 拡張機能の App Builder アプリケーション
1. 拡張機能の変更が許容できる場合 __確定__ レビュー。 これにより、直ちに、Adobe組織内のすべてのAEMas a Cloud Serviceオーサーサービスに拡張機能が挿入されます。

拡張機能のリクエストが承認されると、直ちにAEMas a Cloud Serviceのオーサーサービスで拡張機能がアクティブになります。

## 拡張機能の削除

![拡張機能の削除](./assets/deploy/revoke.png)

拡張機能を削除するには、Exchange から取り消す（または削除する）必要があります。Adobe 拡張機能が取り消されると、すべてのAEMas a Cloud Serviceオーサーサービスから削除されます。

1. にログインします。 [Adobe交換](https://exchange.adobe.com/)
1. に移動します。 __管理__ > __App Builder アプリ__
1. __失効__ 削除する拡張機能
