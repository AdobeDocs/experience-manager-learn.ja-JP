---
title: フロントエンドパイプラインを使用したデプロイ
description: フロントエンドリソースを構築し、AEM as a Cloud Serviceの組み込み CDN にデプロイするフロントエンドパイプラインを作成して実行する方法を説明します。
version: Cloud Service
type: Tutorial
feature: AEM Project Archetype, Cloud Manager, CI-CD Pipeline
topic: Content Management, Development, Development, Architecture
role: Developer, Architect, Admin
level: Intermediate
kt: 10689
mini-toc-levels: 1
index: y
recommendations: noDisplay, noCatalog
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---


# フロントエンドパイプラインを使用したデプロイ

この章では、Cloud Manager でフロントエンドパイプラインを作成し、Adobeします。 次の場所からファイルを構築するだけです： `ui.frontend` モジュールを使用し、AEM as a Cloud Serviceの組み込み CDN にデプロイします。 こうして、  `/etc.clientlibs` フロントエンドリソース配信に基づくもの。


## 目的 {#objectives}

* フロントエンドパイプラインを作成し、実行します。
* 次の場所からフロントエンドリソースが配信されていないことを確認します。 `/etc.clientlibs` しかし、次で始まる新しいホスト名から `https://static-`

## フロントエンドパイプラインの使用

>[!VIDEO](https://video.tv.adobe.com/v/3409420?quality=12&learn=on)

## 前提条件 {#prerequisites}

これは複数のパートから成るチュートリアルで、 [標準AEMプロジェクトを更新](./update-project.md) が完了しました。

次の条件を満たしていることを確認します。 [Cloud Manager でパイプラインを作成およびデプロイする権限](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/requirements/users-and-roles.html?lang=en#role-definitions) および [AEMas a Cloud Service環境へのアクセス](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-environments.html).

## 既存のパイプライン名を変更

次の既存のパイプラインの名前を変更 __開発環境にデプロイ__ から  __FullStack WKND 開発環境へのデプロイ__ に移動して、 __設定__ タブの __実稼動以外のパイプライン名__ フィールドに入力します。 これは、名前を見るだけで、パイプラインがフルスタックかフロントエンドかを明確にするためです。

![パイプライン名を変更](assets/fullstack-wknd-deploy-dev-pipeline.png)


また、 __ソースコード__ 「 」タブで、「リポジトリ」フィールドと「 Git ブランチ」フィールドの値が正しいこと、およびブランチにフロントエンドパイプライン契約の変更があることを確認します。

![ソースコード設定パイプライン](assets/fullstack-wknd-source-code-config.png)


## フロントエンドパイプラインの作成

宛先 __のみ__ 次の場所からフロントエンドリソースを作成してデプロイする `ui.frontend` モジュールで、次の手順を実行します。

1. Cloud Manager UI で、 __パイプライン__ セクションで、 __追加__ ボタンをクリックし、「 __実稼動以外のパイプラインを追加__ ( または __実稼動パイプラインを追加__) を、デプロイ先のAEMas a Cloud Service環境に基づいて作成します。

1. 内 __実稼動以外のパイプラインを追加__ ダイアログ、 __設定__ ステップ、 __デプロイメントパイプライン__ オプション、名前を __開発環境へのフロントエンド WKND デプロイ__&#x200B;をクリックし、 __続行__

![フロントエンドパイプライン設定の作成](assets/create-frontend-pipeline-configs.png)

1. の一部として __ソースコード__ ステップ、 __フロントエンドコード__ オプションを選択し、次の場所から環境を選択します。 __適格なデプロイメント環境__. 内 __ソースコード__ 「リポジトリ」と「 Git ブランチ」のフィールド値が正しいこと、およびブランチにフロントエンドパイプライン契約が変更されていることを確認します。
および __最も重要な__ の __コードの場所__ 値が次の値を持つフィールド `/ui.frontend` 最後に、「 __保存__.

![フロントエンドパイプラインソースコードの作成](assets/create-frontend-pipeline-source-code.png)


## デプロイメント順序

* 最初に、新しく名前が変更された __FullStack WKND 開発環境へのデプロイ__ パイプラインを使用して WKND clientlib ファイルをAEMリポジトリから削除します。 さらに最も重要なのは、 __Sling 設定__ ファイル (`SiteConfig`, `HtmlPageItemsConfig`) をクリックします。

![スタイルが設定されていない WKND サイト](assets/unstyled-wknd-site.png)

>[!WARNING]
>
>次の後、 __FullStack WKND 開発環境へのデプロイ__ パイプライン完了が完了しました __文様のない__ WKND サイトが壊れているように見える場合があります。 1 つのフルスタックパイプラインからフロントエンドパイプラインへの最初の切り替え中に、停止またはデプロイを奇数時間に計画してください。これは、1 回のフルスタックパイプラインの使用からフロントエンドパイプラインへの初期切り替え中に計画する必要があります。


* 最後に、 __開発環境へのフロントエンド WKND デプロイ__ パイプラインはビルドのみ `ui.frontend` モジュールを使用し、フロントエンドリソースを直接 CDN にデプロイします。

>[!IMPORTANT]
>
>君は __文様のない__ WKND サイトが正常に戻り、今回は __フロントエンド__ パイプラインの実行は、フルスタックパイプラインよりもはるかに高速でした。

## スタイルの変更と新しい配信パラダイムの検証

* WKND サイトの任意のページを開くと、テキストの色が表示されます。 __Adobe赤__ フロントエンドリソース (CSS、JS) ファイルは、CDN から配信されます。 リソースリクエストのホスト名はで始まります。 `https://static-pXX-eYY.p123-e456.adobeaemcloud.com/$HASH_VALUE$/theme/site.css` 同様に、 `HtmlPageItemsConfig` ファイル。


![新しくスタイル設定された WKND サイト](assets/newly-styled-wknd-site.png)



>[!TIP]
>
>この `$HASH_VALUE$` ここでは、 __開発環境へのフロントエンド WKND デプロイ__  パイプラインの __コンテンツハッシュ__ フィールドに入力します。 AEMにフロントエンドリソースの CDN URL が通知され、値は `/conf/wknd/sling:configs/com.adobe.cq.wcm.core.components.config.HtmlPageItemsConfig/jcr:content` under __prefixPath__ プロパティ。


![ハッシュ値の相関関係](assets/hash-value-correlartion.png)



## おめでとうございます。 {#congratulations}

これで、WKND Sites プロジェクトの「ui.frontend」モジュールを構築しデプロイするフロントエンドパイプラインを作成し、実行し、検証しました。 これで、フロントエンドチームは、AEMプロジェクトのライフサイクル全体を超えて、サイトのデザインとフロントエンドの動作をすばやく繰り返し実行できます。

## 次の手順 {#next-steps}

次の章では、 [注意点](considerations.md)を使用している場合は、フロントエンドおよびバックエンドの開発プロセスに与える影響を確認します。
