---
title: HTM5フォーム送信時のトリガーAEMワークフロー
seo-title: HTML5フォーム送信時のトリガーAEMワークフロー
description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
seo-description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームをトリガーAEMワークフローに送信する
feature: 'モバイルフォーム '
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 1%

---


# この使用例をシステムで使用する

>[!NOTE]
>
>サンプルアセットがシステムで動作するためには、AEMオーサーインスタンスとパブリッシュインスタンスがそれぞれポート4502と4503で動作していることを前提としています。 また、AEMオーサーが`admin`/`admin`経由でアクセスできると想定されます。 ポート番号または管理者パスワードを変更した場合、これらのサンプルアセットは機能しません。 提供されたサンプルコードを使用して、独自のアセットを作成する必要があります。

ローカルシステムでこの使用例を機能させるには、次の手順に従います。

* ポート4502のAEMオーサーインスタンスと、ポート4503のAEMパブリッシュインスタンスをインストールします。
* [AEM Formsでのサービスユーザーを使用した開発の手順に従います](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)。サービスユーザーを作成し、AEMオーサーインスタンスとパブリッシュインスタンスにバンドルをデプロイしてください。
* [osgi設定を開きま ](http://localhost:4503/system/console/configMgr)す。
* **Apache Sling Referrer Filter**&#x200B;を検索します。 「 Allow Empty 」チェックボックスがオンになっていることを確認します。
* [カスタムAEMormDocumentServiceバンドルをデプロイします](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。このバンドルは、AEMパブリッシュインスタンスにデプロイする必要があります。このバンドルには、モバイルフォームからインタラクティブPDFを生成するコードが含まれます。
* [この記事に関連するアセットをダウンロードして解凍します。](assets/offline-pdf-submission-assets.zip) 次の情報が表示されます。
   * **offline-submission-profile.zip**  — このAEMパッケージには、インタラクティブpdfをローカルファイルシステムにダウンロードできるカスタムプロファイルが含まれています。このパッケージをAEMパブリッシュインスタンスにデプロイします。
   * **xdp-form-and-workflow.zip**  — このAEMパッケージには、content/pdfsubmissionsノードに設定されたXDP、サンプルワークフロー、ランチャーが含まれています。このパッケージをAEMオーサーインスタンスとパブリッシュインスタンスの両方にデプロイします。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  — ほとんどの作業を実行するAEMバンドルです。このバンドルには、`/bin/startworkflow`にマウントされたサーブレットが含まれます。 このサーブレットは、送信されたフォームデータをAEMリポジトリの`/content/pdfsubmissions`ノードに保存します。 このバンドルをAEMオーサーインスタンスとパブリッシュインスタンスの両方にデプロイします。
* [モバイルフォームのプレビュー](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 複数のフィールドに入力し、ツールバーのボタンをクリックしてインタラクティブPDFをダウンロードします。
* Acrobatを使用してダウンロードしたPDFに入力し、送信ボタンを押します。
* 成功メッセージが表示されます
* AEMオーサーインスタンスに管理者としてログインします。
* [AEM Inboxを確認する](http://localhost:4502/aem/inbox)
* 送信されたPDFを確認する作業項目が必要です

>[!NOTE]
>
>パブリッシュインスタンス上で実行されるサーブレットにPDFを送信する代わりに、一部のお客様は、Tomcatなどのサーブレットコンテナにサーブレットをデプロイしています。 このチュートリアルでは、お客様が使用するトポロジに応じて、PDF送信を処理するために、パブリッシュインスタンスにデプロイされたサーブレットを使用します。

