---
title: HTM5 フォーム送信でのトリガーAEMのワークフロー — 使用例を使用する
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: モバイルフォームをオフラインモードで入力し続け、モバイルフォームをトリガーAEMワークフローに送信します
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 1%

---

# この使用例をシステムで動作させる

>[!NOTE]
>
>サンプルアセットがシステム上で動作するようにするには、AEM オーサーインスタンスとパブリッシュインスタンスがそれぞれポート 4502 と 4503 で動作していることを前提としています。 また、AEM オーサーがを介してアクセスできると想定されます。 `admin`/`admin`. ポート番号または管理者パスワードが変更されている場合、これらのサンプルアセットは機能しません。 提供されたサンプルコードを使用して独自のアセットを作成する必要があります。

ローカルシステムでこのユースケースを機能させるには、次の手順に従います。

* ポート 4502 の AEM オーサーインスタンスと、ポート 4503 の AEM パブリッシュインスタンスをインストールします。
* [AEM Formsでのサービスユーザーを使用した開発で指定されている手順に従います](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). 必ずサービスユーザーを作成し、AEM オーサーインスタンスとパブリッシュインスタンスにバンドルをデプロイしてください。
* [OSGi 設定を開きます。 ](http://localhost:4503/system/console/configMgr).
* を検索  **Apache Sling Referrer Filter**. 「 Allow Empty 」チェックボックスがオンになっていることを確認します。
* [カスタム AEMormDocumentService バンドルをデプロイします](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)このバンドルは、AEM パブリッシュインスタンスにデプロイする必要があります。 このバンドルには、モバイルフォームからインタラクティブPDFを生成するコードが含まれています。
* [この記事に関連するアセットをダウンロードして展開します。](assets/offline-pdf-submission-assets.zip) 次の情報が表示されます
   * **offline-submission-profile.zip**  — このAEMパッケージには、インタラクティブ pdf をローカルファイルシステムにダウンロードできるカスタムプロファイルが含まれています。 このパッケージを AEM パブリッシュインスタンスにデプロイします。
   * **xdp-form-and-workflow.zip**  — このAEMパッケージには、ノード content/pdfsubmissions に設定された XDP、サンプルワークフロー、ランチャーが含まれています。 このパッケージを AEM オーサーインスタンスとパブリッシュインスタンスの両方にデプロイします。
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  — これは、ほとんどの機能を実行するAEMバンドルです。 このバンドルには、 `/bin/startworkflow`. このサーブレットは、送信されたフォームデータを以下に保存します。 `/content/pdfsubmissions` AEMリポジトリのノード。 このバンドルを AEM オーサーインスタンスとパブリッシュインスタンスの両方にデプロイします。
* [モバイルフォームのプレビュー](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* 複数のフィールドに入力し、ツールバーのボタンをクリックしてインタラクティブPDFをダウンロードします。
* Acrobatを使用してダウンロードしたPDFに入力し、送信ボタンを押します。
* 成功メッセージが表示されます
* 管理者として AEM オーサーインスタンスにログインします。
* [AEM Inbox を確認する](http://localhost:4502/aem/inbox)
* 送信した項目を確認する作業項目が必要です。PDF

>[!NOTE]
>
>一部のお客様は、パブリッシュインスタンスで実行するサーブレットにPDFを送信する代わりに、Tomcat などのサーブレットコンテナにサーブレットをデプロイしています。 これはすべて、お客様が使用するトポロジによって異なります。このチュートリアルでは、パブリッシュインスタンスにデプロイされたサーブレットを使用して、pdf 送信を処理します。
