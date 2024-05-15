---
title: AEM as a Cloud Service 用の AEM ヘッドレスのクイックセットアップ
description: AEM ヘッドレスのクイックセットアップでは、WKND サイトのサンプルプロジェクトのコンテンツと、AEM ヘッドレス GraphQL API を介してコンテンツを使用する React アプリを使用して、AEM ヘッドレスを操作します。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
duration: 781
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 100%

---

# AEM as a Cloud Service 用の AEM ヘッドレスのクイックセットアップ

AEM ヘッドレスのクイックセットアップでは、WKND サイトサンプルプロジェクトのコンテンツと、AEM ヘッドレス GraphQL API を介してコンテンツを使用するサンプルの React アプリ（SPA）を使用して、AEM ヘッドレスを操作できます。

## 前提条件

このクイックセットアップに行うには、以下が必要です。

+ AEM as a Cloud Service サンドボックス環境（できれば開発環境）
+ AEM as a Cloud Service および Cloud Manager へのアクセス
   + AEM as a Cloud Serviceへの __AEM 管理者__&#x200B;アクセス権
   + __Cloud Manager - Cloud Manager へのデプロイメントマネージャー__&#x200B;のアクセス権 
+ 以下のツールをローカルにインストールする必要があります。
   + [Node.js v18](https://nodejs.org/ja/)
   + [Git](https://git-scm.com/)
   + IDE（例：[Microsoft® Visual Studio Code](https://code.visualstudio.com/)）

## 1. Cloud Manager Git リポジトリの作成

まず、WKND サイトのデプロイに使用する Cloud Manager Git リポジトリを作成します。 WKND サイトは、コンテンツ（コンテンツフラグメント）と、クイックセットアップの React アプリで使用される GraphQL AEM エンドポイントを含むサンプルの AEM web サイトプロジェクトです。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com) に移動します
1. このクイックセットアップに使用する AEM as a Cloud Service 環境を含む Cloud Manager __プログラム__&#x200B;を選択します。
1. WKND サイトプロジェクト用の Git リポジトリを作成します
   1. 上部ナビゲーションで「__リポジトリ__」を選択します 
   1. 上部のアクションバーで「__リポジトリを追加__」を選択します。
   1. 新しい Git リポジトリに「`aem-headless-quick-setup-wknd`」という名前を付けます。 
      + Git リポジトリ名は、Adobe 組織ごとに一意である必要があります。
   1. 「__保存__」を選択 して、Git リポジトリが初期化されるのを待ちます。

## 2.サンプル WKND サイトプロジェクトの Cloud Manager Git リポジトリへのプッシュ

Cloud Manager Git リポジトリを作成したら、WKND サイトプロジェクトのソースコードを GitHub からコピーし、Cloud Manager Git リポジトリにプッシュします。 Cloud Manager で、WKND サイトプロジェクトを AEM as a Cloud Service 環境にデプロイします。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. コマンドラインから、サンプルの WKND サイトプロジェクトのソースコードを GitHub からコピーします

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Cloud Manager Git リポジトリをリモートリポジトリとして追加します
   1. 上部ナビゲーションの「__リポジトリ__」を選択します
   1. 上部のアクションバーの「__リポジトリ情報にアクセス__」を選択します。
   1. 「__リモートを Git リポジトリに追加__」にあるコマンドを コマンドラインから実行します。

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. サンプルプロジェクトのソースコードをローカル Git リポジトリから Cloud Manager Git リポジトリにプッシュします。

   ```shell
   $ git push adobe main:main
   ```

   資格情報の入力を求められたら、__ユーザー名__&#x200B;および&#x200B;__パスワード__&#x200B;を Cloud Manager の&#x200B;__リポジトリ情報__&#x200B;モーダルから指定します。

## 3. WKND サイトの AEM as a Cloud Service へのデプロイ

WKND サイトプロジェクトを Cloud Manager Git リポジトリにプッシュすると、Cloud Manager パイプラインを使用して AEM as a Cloud Service にデプロイすることはできません。

WKND サイトプロジェクトには、React アプリが AEM ヘッドレス GraphQL API を介して消費するサンプルコンテンツが用意されています。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. __実稼動以外のデプロイメントパイプライン__&#x200B;を新しい Git リポジトリに追加します。
   1. 上部ナビゲーションの「__パイプライン__」を選択します。
   1. 上部のアクションバーの「__パイプラインを追加__」を選択します。
   1. 「__設定__」タブで、次の設定を行います。
      1. 「__デプロイメントパイプライン__」オプションを選択します。
      1. __実稼動以外のパイプライン名__&#x200B;を `Dev Deployment pipeline` に設定します。
      1. __デプロイメントトリガー／Git の変更時__&#x200B;を選択します 
      1. __重要な指標の失敗動作／今すぐ続行__&#x200B;を選択します
      1. 「__続行__」を選択します
   1. 「__ソースコード__」タブで、以下の操作を行います
      1. 「__フルスタックコード__」オプションを選択します
      1. 「__適格なデプロイメント環境__」選択ボックスから「__AEM as a Cloud Service 開発環境__」を選択します
      1. 「__リポジトリ__」選択ボックスで `aem-headless-quick-setup-wknd` を選択します
      1. 「__Git ブランチ__」選択ボックスで `main` を選択します
      1. 「__保存__」を選択します
1. __開発デプロイメントパイプライン__&#x200B;を実行します。
   1. 上部ナビゲーションの「__概要__」を選択します
   1. 「__パイプライン__」セクションで、新しく作成した&#x200B;__開発デプロイメントパイプライン__&#x200B;を見つけます
   1. パイプラインエントリの右側にある「__...__」を選択します
   1. 「__実行__」を選択し、モーダルで確認します
   1. 実行中のパイプラインの右側にある「__...__」を選択します
   1. 「__詳細を表示__」を選択 します
1. パイプラインの実行の詳細から、正常に完了するまで進行状況を監視します。 パイプラインの実行には 30～40 分かかります

## 4. WKND React アプリのダウンロードと実行

WKND サイトプロジェクトのコンテンツでブートストラップを行う AEMas a Cloud Service を使って、AEM ヘッドレス GraphQL API を介して WKND サイトのコンテンツを使用するサンプルの WKND React アプリをダウンロードして起動します。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. コマンドラインから、GitHub の React アプリのソースコードをコピーします。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. IDE で `~/Code/aem-guides-wknd-graphql/react-app` フォルダーを開きます。 
1. IDE で `.env.development` ファイルを開きます。
1. `REACT_APP_HOST_URI` プロパティから AEM as a Cloud Service __Publish__&#x200B;サービスのホスト URI  をポイントします。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   AEM as a Cloud Service Publish サービスのホスト URI を見つけるには

   1. Cloud Manager で、上部ナビゲーションの「__環境__」を選択します。
   1. 「__開発__」環境を選択します
   1. __Publish サービス（AEM および Dispatcher）__ リンクの「__環境セグメント__」テーブルを見つけます
   1. リンクのアドレスをコピーして、AEM as a Cloud Service Publish サービスの URI として使用します

1. IDE で 変更を `.env.development` に保存します
1. コマンドラインから、React アプリを実行します。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. ローカルで実行される React アプリが [http://localhost:3000](http://localhost:3000) で開始し、AEM ヘッドレスの GraphQL API を使用して AEM as a Cloud Service から取得されるアドベンチャーのリストを表示します。

## 5. AEM でのコンテンツの編集

AEM ヘッドレス GraphQL API のコンテンツに接続して消費するサンプル WKND React アプリを使用して、AEM オーサーサービスのコンテンツを作成し、React アプリのエクスペリエンスがどのように更新されるかを確認します。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. AEM as a Cloud Service オーサーサービスにログインします
1. __Assets／ファイル／WKND 共有／英語／アドベンチャー__&#x200B;に移動します
1. __ユタ州南部のサイクリング__&#x200B;フォルダーを開きます
1. __ユタ州南部のサイクリング__ コンテンツフラグメントを選択し、上部のアクションバーの「__編集__」を選択します
1. 例えば、以下のようにコンテンツフラグメントの一部のフィールドを更新します。
   + タイトル：`Cycling Utah's National Parks`
   + 旅の期間： `6 Days`
   + 難易度： `Intermediate`
   + 価格：`3500`
   + プライマリ画像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 上部のアクションバーで「__保存__」を選択します
1. 上部のアクションバーの「__...__」から「__クイック公開__」を選択します
1. [http://localhost:3000](http://localhost:3000) で実行中の React アプリを更新します
1. React アプリで、更新後のサイクリングアドベンチャーを選択し、コンテンツフラグメントに加えたコンテンツの変更を確認します。

1. AEM オーサーサービスでも、同じ方法を使用します。
   1. 既存の Adventure コンテンツフラグメントを非公開にし、React アプリエクスペリエンスから削除されたことを確認します。
   1. 新しいアドベンチャーコンテンツフラグメントを作成して公開し、React アプリのエクスペリエンスに表示されることを確認します。

   >[!TIP]
   >
   > 新しいコンテンツフラグメントの作成と公開や、既存のコンテンツフラグメントの非公開に不慣れな場合は、上記のスクリーンキャストをご覧ください。

## これで完了です。

おめでとうございます。AEM ヘッドレスを正常に使用して React アプリを強化できました。

React アプリが AEM as a Cloud Service 内のコンテンツをどのように消費するかを詳しく理解するには、[AEM ヘッドレスチュートリアル](../multi-step/overview.md)を参照してください。このチュートリアルでは、AEM でコンテンツフラグメントがどのように作成されるかと、この React アプリがコンテンツを JSON としてどのように消費するかを説明します。

### 次の手順

+ [AEM ヘッドレスの開始（チュートリアル）](../multi-step/overview.md)
