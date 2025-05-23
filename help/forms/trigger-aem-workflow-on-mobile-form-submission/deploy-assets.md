---
title: HTML5 フォーム送信での AEM ワークフローのトリガー - ユースケースを動作させる
description: ローカルシステムへのサンプルアセットのデプロイ
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 9417235f-2e8d-45c7-86eb-104478a69a19
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '392'
ht-degree: 100%

---

# このユースケースをお使いのシステムで動作させる

>[!NOTE]
>
>サンプルアセットがシステム上で動作するには、AEM Forms オーサーインスタンスと AEM Forms パブリッシュインスタンスにアクセスできることが前提となります。

ローカルシステムでこのユースケースを動作させるには、次の手順に従います。

## AEM Forms オーサーインスタンスに以下をデプロイする

* [MobileFormToWorkflow バンドルのインストール](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [サービスユーザーバンドルを使用して開発をデプロイする](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=ja)
configMgr を使用して、Apache Sling Service User Mapper Service に次のエントリを追加します

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* [configMgr](http://localhost:4502/system/console/configMg) を使用して、AEM Server の資格情報設定のフォルダー名を指定することで、フォーム送信を別のフォルダーに格納できます。フォルダーを変更する場合は、ワークフロー **ReviewSubmittedPDF** をトリガーするランチャーをフォルダー上に作成してください。

![config-author](assets/author-config.png)
* [パッケージマネージャーを使用して、サンプル xdp とワークフローパッケージを読み込みます](assets/xdp-form-and-workflow.zip)。


## 次のアセットをパブリッシュインスタンスにデプロイする

* [MobileFormToWorkflow バンドルのインストール](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* オーサーインスタンスのユーザー名とパスワードおよび **AEM リポジトリ内の既存の場所**&#x200B;を指定し、[configMgr](http://localhost:4503/system/console/configMgr) を使用して送信されたデータを AEM サーバーの資格情報に保存します。AEM ワークフローサーバー上のエンドポイントの URL はそのままにすることができます。これは、指定されたノードの送信からデータを抽出して保存するエンドポイントです。
  ![publish-config](assets/publish-config.png)

* [サービスユーザーバンドルを使用した開発のデプロイ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=ja)
* [OSGi 設定を開きます](http://localhost:4503/system/console/configMgr)。
* **Apache Sling Referrer Filter** を検索します。「空白を許可」チェックボックスがオンになっていることを確認します。


## ソリューションのテスト

* オーサーインスタンスにログイン
* [w9.xdp の詳細プロパティを編集します](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp)。送信 URL とレンダリングプロファイルが以下に示すように正しく設定されていることを確認します。
  ![xdp-advanced-properties](assets/mobile-form-properties.png)

* w9.xdp の公開
* パブリッシュインスタンスにログイン
* [w9 フォームのプレビュー](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* フォームフィールドに入力し、フォームを送信します
* 管理者として AEM オーサーインスタンスにログインします。
* [AEM インボックスを確認する](http://localhost:4502/aem/inbox)
* 送信された PDF 項目を確認する作業項目が必要です。

>[!NOTE]
>
>一部の顧客は、パブリッシュインスタンスで実行するサーブレットに PDF を送信する代わりに、Tomcat などのサーブレットコンテナにサーブレットをデプロイしています。 これはすべて、顧客が使用するトポロジによって異なります。このチュートリアルでは、パブリッシュインスタンスにデプロイされたサーブレットを使用して、フォーム送信を処理します。
