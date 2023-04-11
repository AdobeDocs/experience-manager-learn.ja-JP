---
title: AEM AEM as a Cloud Serviceのヘッドレスクイックセットアップ
description: AEMヘッドレスのクイックセットアップでは、WKND Site サンプルプロジェクトのコンテンツを使用してAEMヘッドレスを操作し、AEMヘッドレスGraphQL API を介してコンテンツを使用する React App を使用できます。
version: Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
kt: 9442
thumbnail: 339073.jpg
exl-id: 62e807b7-b1a4-4344-9b1e-2c626b869e10
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '1081'
ht-degree: 1%

---

# AEM AEM as a Cloud Serviceのヘッドレスクイックセットアップ

AEMヘッドレスのクイックセットアップでは、WKND Site サンプルプロジェクトのコンテンツと、AEM Headless GraphQL API を介してコンテンツを使用するサンプルの React App(SPA) を使用して、AEMヘッドレスを操作できます。

## 前提条件

このクイックセットアップに従うには、次の操作が必要です。

+ AEMas a Cloud Serviceサンドボックス環境（できれば開発）
+ AEM as a Cloud Serviceおよび Cloud Manager へのアクセス
   + __AEM Administrator__ AEM as a Cloud Serviceへのアクセス
   + __Cloud Manager — デプロイメントマネージャー__ Cloud Manager へのアクセス
+ 以下のツールをローカルにインストールする必要があります。
   + [Node.js v18](https://nodejs.org/ja/)
   + [Git](https://git-scm.com/)
   + IDE( 例： [Microsoft® Visual Studio Code](https://code.visualstudio.com/))

## 1. Cloud Manager Git リポジトリを作成する

まず、WKND サイトのデプロイに使用する Cloud Manager Git リポジトリを作成します。 WKND サイトは、コンテンツ（コンテンツフラグメント）と、クイックセットアップの React App で使用されるGraphQL AEMエンドポイントを含む、サンプルのAEM Web サイトプロジェクトです。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339073?quality=12&learn=on)

1. に移動します。 [https://my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com)
1. Cloud Manager を選択します。 __プログラム__ このクイックセットアップに使用するAEMas a Cloud Service環境を含む
1. WKND サイトプロジェクト用の Git リポジトリを作成する
   1. 選択 __リポジトリ__ 上部ナビゲーション
   1. 選択 __リポジトリを追加__ 上部のアクションバー
   1. 新しい Git リポジトリに次の名前を付けます。 `aem-headless-quick-setup-wknd`
      + Git リポジトリ名は、組織ごとに一意である必要があり、Adobeの組織ごとに一意です。
   1. 選択 __保存__ Git リポジトリが初期化されるのを待ちます。

## 2.サンプル WKND サイトプロジェクトを Cloud Manager Git リポジトリにプッシュします

Cloud Manager Git リポジトリを作成したら、WKND Site プロジェクトのソースコードを GitHub からコピーし、Cloud Manager Git リポジトリにプッシュします。 Cloud Manager で、WKND サイトプロジェクトにアクセスし、AEMas a Cloud Service環境にデプロイできます。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339074?quality=12&learn=on)

1. コマンドラインから、サンプルの WKND Site プロジェクトのソースコードを GitHub からコピーします

   ```shell
   $ mkdir -p ~/Code
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd.git
   ```

1. Cloud Manager Git リポジトリをリモートリポジトリとして追加する
   1. 選択 __リポジトリ__ 上部ナビゲーション
   1. 選択 __リポジトリ情報にアクセス__ 上部のアクションバーから
   1. 次の場所にコマンドを実行 __リモートを Git リポジトリに追加する__ コマンドラインから

      ```shell
      $ cd aem-guides-wknd
      $ git remote add adobe https://git.cloudmanager.adobe.com/<YOUR ADOBE ORGANIZATION>/aem-headless-quick-setup-wknd/
      ```

1. サンプルプロジェクトのソースコードをローカル Git リポジトリーから Cloud Manager Git リポジトリーにプッシュします。

   ```shell
   $ git push adobe main:main
   ```

   資格情報の入力を求められたら、 __ユーザー名__ および __パスワード__ Cloud Manager の __リポジトリ情報__ モーダルです。

## 3. WKND サイトをAEM as a Cloud Serviceにデプロイ

WKND サイトプロジェクトを Cloud Manager Git リポジトリにプッシュすると、Cloud Manager パイプラインを使用してAEM as a Cloud Serviceにデプロイすることはできません。

WKND Site プロジェクトには、React アプリがAEMヘッドレスGraphQL API を介して消費するサンプルコンテンツが用意されています。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339075?quality=12&learn=on)

1. の添付 __実稼動以外のデプロイメントパイプライン__ を新しい Git リポジトリに追加します。
   1. 選択 __パイプライン__ 上部ナビゲーション
   1. 選択 __パイプラインを追加__ 上部のアクションバーから
   1. の __設定__ タブ
      1. 選択 __デプロイメントパイプライン__ オプション
      1. を __実稼動以外のパイプライン名__ から `Dev Deployment pipeline`
      1. 選択 __デプロイメントトリガー/Git の変更時__
      1. 選択 __重要な指標の失敗動作 > 今すぐ続行__
      1. 選択 __続行__
   1. の __ソースコード__ タブ
      1. 選択 __フルスタックコード__ オプション
      1. を選択します。 __AEMas a Cloud Service開発環境__ から __適格なデプロイメント環境__ 選択ボックス
      1. 選択 `aem-headless-quick-setup-wknd` 内 __リポジトリ__ 選択ボックス
      1. 選択 `main` から __Git ブランチ__ 選択ボックス
      1. 選択 __保存__
1. を実行します。 __開発デプロイメントパイプライン__
   1. 選択 __概要__ 上部ナビゲーション
   1. 新しく作成した __開発デプロイメントパイプライン__ 内 __パイプライン__ セクション
   1. を選択します。 __...__ をパイプラインエントリの右側に追加します。
   1. 選択 __実行__&#x200B;をクリックし、モーダルでを確認します。
   1. を選択します。 __...__ を実行中のパイプラインの右側に移動します。
   1. 選択 __詳細を表示__
1. パイプラインの実行の詳細から、正常に完了するまで進行状況を監視します。 パイプラインの実行には 30 ～ 40 分かかる必要があります。

## 4. WKND React アプリをダウンロードして実行します。

WKND Site プロジェクトのコンテンツでAEMas a Cloud Service的にブートストラップ処理したら、AEMヘッドレスGraphQL API を介して WKND Site のコンテンツを使用するサンプルの WKND React App をダウンロードして、起動します。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339076?quality=12&learn=on)

1. コマンドラインから、React App のソースコードを GitHub からクローンします。

   ```shell
   $ cd ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. フォルダーを開く `~/Code/aem-guides-wknd-graphql/react-app` を IDE に追加します。
1. IDE で、ファイルを開きます。 `.env.development`.
1. AEMas a Cloud Serviceを指す __公開__ から取得したサービスのホスト URI  `REACT_APP_HOST_URI` プロパティ。

   ```plain
   REACT_APP_HOST_URI=https://publish-pXXXX-eYYYY.adobeaemcloud.com
   ...
   ```

   AEMas a Cloud Serviceパブリッシュサービスのホスト URI を検索するには：

   1. Cloud Manager で、 __環境__ 上部ナビゲーション
   1. 選択 __開発__ 環境
   1. を __パブリッシュサービス (AEMおよび Dispatcher)__ リンク __環境セグメント__ 表
   1. リンクのアドレスをコピーして、AEMas a Cloud Serviceパブリッシュサービスの URI として使用します

1. IDE で、変更を `.env.development`
1. コマンドラインから、React App を実行します。

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/react-app
   $ npm install
   $ npm start
   ```

1. ローカルで実行される React アプリは、次の時点で開始します。 [http://localhost:3000](http://localhost:3000) およびは、AEMヘッドレスのGraphQL API を使用してAEM as a Cloud Serviceから提供されるアドベンチャの一覧を表示します。

## 5. AEMでコンテンツを編集する

AEMヘッドレスGraphQL API のコンテンツに接続し、利用するサンプル WKND React アプリを使用して、AEM オーサーサービスのコンテンツを作成し、React アプリのエクスペリエンスがどのように更新されるかを確認します。

_手順のスクリーンキャスト_
>[!VIDEO](https://video.tv.adobe.com/v/339077?quality=12&learn=on)

1. AEM as a Cloud Service Author サービスにログインします。
1. に移動します。 __アセット/ファイル/WKND 共有/英語/冒険__
1. を開きます。 __サイクリングサザンユタ__ フォルダー
1. を選択します。 __サイクリングサザンユタ__ コンテンツフラグメントを選択し、 __編集__ 上部のアクションバーから
1. コンテンツフラグメントの一部のフィールドを更新します。例：
   + タイトル: `Cycling Utah's National Parks`
   + 旅行の長さ： `6 Days`
   + 難易度： `Intermediate`
   + 価格: `3500`
   + プライマリ画像： `/content/dam/wknd-shared/en/activities/cycling/mountain-biking.jpg`
1. 選択 __保存__ 上部のアクションバー
1. 選択 __クイック公開__ 上部のアクションバーの __...__
1. で実行中の React アプリを更新する [http://localhost:3000](http://localhost:3000).
1. React アプリで、更新されたサイクリングのアドベンチャーを選択し、コンテンツフラグメントに加えられたコンテンツの変更を確認します。

1. AEM オーサーサービスでも、同じ方法を使用します。
   1. 既存のアドベンチャーコンテンツフラグメントを非公開にし、React App エクスペリエンスから削除されたことを確認します。
   1. 新しいアドベンチャーコンテンツフラグメントを作成して公開し、React App エクスペリエンスに表示されることを確認します。

   >[!TIP]
   >
   > 新しいコンテンツフラグメントの作成と公開、または既存のコンテンツフラグメントの非公開に慣れていない場合は、上記のスクリーンキャストをご覧ください。

## おめでとうございます。

おめでとうございます。AEMヘッドレスを使用して React アプリを強化できました。

React App がAEM as a Cloud Serviceからコンテンツをどのように消費するかを詳しく理解するには、 [AEMヘッドレスチュートリアル](../multi-step/overview.md). このチュートリアルでは、AEMでのコンテンツフラグメントの作成方法と、この React App が JSON としてコンテンツを使用する方法について説明します。

### 次のステップ

+ [AEMヘッドレスチュートリアルの開始](../multi-step/overview.md)
