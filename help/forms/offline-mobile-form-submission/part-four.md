---
title: HTM5 フォーム送信での AEM ワークフローのトリガー - ユースケースを動作させる
description: オフラインモードでのモバイルフォームへの入力の継続とモバイルフォームの送信による AEM ワークフローのトリガー
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
duration: 90
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 100%

---

# このユースケースをシステムで動作させる

>[!NOTE]
>
>サンプルアセットがシステム上で動作するようにするには、AEM オーサーインスタンスとパブリッシュインスタンスがそれぞれポート 4502 と 4503 で動作していることを前提としています。 また、AEM オーサーが `admin`/`admin` を介してアクセスできると想定されます。ポート番号または管理者パスワードが変更されている場合、これらのサンプルアセットは機能しません。提供されたサンプルコードを使用して独自のアセットを作成する必要があります。

ローカルシステムでこのユースケースを機能させるには、次の手順に従います。

* ポート 4502 の AEM オーサーインスタンスと、ポート 4503 の AEM パブリッシュインスタンスをインストールする
* [AEM Forms でのサービスユーザーを使用した開発で指定されている手順に従います](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=ja)。必ずサービスユーザーを作成し、AEM オーサーインスタンスとパブリッシュインスタンスにバンドルをデプロイしてください。
* [OSGi 設定を開きます](http://localhost:4503/system/console/configMgr)。
* **Apache Sling Referrer Filter** を検索します。「空白を許可」チェックボックスがオンになっていることを確認します。
* [カスタム AEMormDocumentService バンドルをデプロイします](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。このバンドルは、AEM パブリッシュインスタンスにデプロイする必要があります。 このバンドルには、モバイルフォームからインタラクティブ PDF を生成するコードが含まれています。
* [この記事に関連するアセットをダウンロードして展開します。](assets/offline-pdf-submission-assets.zip)次の情報が表示されます
   * **offline-submission-profile.zip** - この AEM パッケージには、インタラクティブ PDF をローカルファイルシステムにダウンロードできるカスタムプロファイルが含まれています。このパッケージを AEM パブリッシュインスタンスにデプロイします。
   * **xdp-form-and-workflow.zip** - この AEM パッケージには、ノード content/pdfsubmissions に設定された XDP、サンプルワークフロー、ランチャーが含まれています。このパッケージを AEM オーサーインスタンスとパブリッシュインスタンスの両方にデプロイします。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** - ほとんどの機能を実行する AEM バンドルです。 このバンドルには、`/bin/startworkflow` にマウントされたサーブレットが含まれています。このサーブレットは、送信されたフォームデータを AEMリポジトリの `/content/pdfsubmissions` ノードに保存します。このバンドルを AEM オーサーインスタンスとパブリッシュインスタンスの両方にデプロイします。
* [モバイルフォームのプレビュー](http://localhost:4503/content/dam/formsanddocuments/shipping-address-addition-updation-form/jcr:content)
* 複数のフィールドに入力し、ツールバーのボタンをクリックしてインタラクティブ PDF をダウンロードします。
* Acrobat を使用してダウンロードした PDF に入力し、「送信」ボタンを押します。
* 成功メッセージが表示されます
* 管理者として AEM オーサーインスタンスにログインします。
* [AEM インボックスを確認する](http://localhost:4502/aem/inbox)
* 送信された PDF 項目を確認する作業項目が必要です。

>[!NOTE]
>
>一部の顧客は、パブリッシュインスタンスで実行するサーブレットに PDF を送信する代わりに、Tomcat などのサーブレットコンテナにサーブレットをデプロイしています。 これはすべて、顧客が使用するトポロジによって異なります。このチュートリアルでは、パブリッシュインスタンスにデプロイされたサーブレットを使用して、PDF 送信を処理します。
