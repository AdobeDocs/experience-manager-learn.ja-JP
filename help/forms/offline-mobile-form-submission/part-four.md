---
title: HTM5フォーム送信でのAEMワークフローのトリガー
seo-title: HTML5フォーム送信時のAEMワークフローのトリガー
description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームを送信してAEMワークフローをトリガーする
seo-description: オフラインモードでモバイルフォームの入力を続け、モバイルフォームを送信してAEMワークフローをトリガーする
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 1%

---


# この使用例をシステムで動作させる

>[!NOTE]
>
>サンプルアセットがシステムで動作するようにするためには、AEM作成者インスタンスと発行インスタンスがそれぞれポート4502と4503で実行されていることを前提としています。 また、AEM作成者は `admin`/からアクセスできると想定してい`admin`ます。 ポート番号または管理者パスワードが変更されている場合、これらのサンプルアセットは機能しません。 付属のサンプルコードを使用して独自のアセットを作成する必要があります。

この使用例をローカルシステムで動作させるには、次の手順に従います。

* ポート4502にAEM作成者インスタンスをインストールし、ポート4503にAEM発行インスタンスをインストールします
* [「AEM Formsのサービスユーザーを使用した開発」で指定されている手順に従い](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html)ます。 サービスユーザーを作成し、AEM Authorおよび発行インスタンスにバンドルをデプロイしてください。
* [osgi設定を開き ](http://localhost:4503/system/console/configMgr)ます。
* 「 **Apache Sling転送者フィルター**」を検索します。 「空白を許可」チェックボックスが選択されていることを確認します。
* [カスタムのAEMFormDocumentService Bundleをデプロイします](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。このバンドルは、AEM発行インスタンスにデプロイする必要があります。 このバンドルには、モバイルフォームからインタラクティブPDFを生成するコードが含まれています。
* [この記事に関連するアセットをダウンロードして解凍します。](assets/offline-pdf-submission-assets.zip) 次の情報が表示されます。
   * **offline-submission-プロファイル.zip** — このAEMパッケージには、インタラクティブpdfをローカルファイルシステムにダウンロードできるカスタムプロファイルが含まれています。 このパッケージをAEM発行インスタンスに展開します。
   * **xdp-form-and-workflow.zip** — このAEMパッケージには、XDP、サンプルワークフロー、ランチャーが含まれており、これはノードcontent/pdfsubmissions上に設定されています。 このパッケージは、AEMオーサーインスタンスと発行インスタンスの両方に展開します。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** — これは、ほとんどの作業を行うAEMバンドルです。 このバンドルには、にマウントされたサーブレットが含まれ `/bin/startworkflow`ます。 このサーブレットは、送信されたフォームデータをAEMリポジトリの `/content/pdfsubmissions` ノードに保存します。 このバンドルは、AEMオーサーインスタンスと発行インスタンスの両方にデプロイします。
* [モバイルフォームのプレビュー](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 複数のフィールドに入力し、ツールバーのボタンをクリックしてインタラクティブPDFをダウンロードします。
* Acrobatを使用して、ダウンロードしたPDFに入力し、送信ボタンを押します。
* 成功のメッセージが表示されます
* AEM作成者インスタンスに管理者としてログイン
* [AEM受信トレイの確認](http://localhost:4502/aem/inbox)
* 送信済みPDFをレビューする作業項目が必要です

>[!NOTE]
>
>PDFをパブリッシュインスタンスで実行するサーブレットに送信する代わりに、Tomcatなどのサーブレットコンテナにサーブレットをデプロイしたお客様もいます。 すべては、お客様が使用するトポロジに依存します。このチュートリアルの目的で、パブリッシュインスタンスにデプロイされたサーブレットを使用して、pdf送信を処理します。

