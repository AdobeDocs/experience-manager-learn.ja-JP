---
title: Forms Workflowの「電子メールの送信」手順の使用
seo-title: Forms Workflowの「電子メールの送信」手順の使用
description: 「電子メールの送信」手順は、AEM Forms6.4で導入されました。この手順を使用すると、添付ファイルの有無に関係なく電子メールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントの設定手順について説明します
seo-description: 「電子メールの送信」手順は、AEM Forms6.4で導入されました。この手順を使用すると、添付ファイルの有無に関係なく電子メールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントの設定手順について説明します
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: workflow
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 3%

---


# Forms Workflow{#using-send-email-step-of-forms-workflow}の「Send Email Step」を使用

「電子メールの送信」手順は、AEM Forms6.4で導入されました。この手順を使用すると、添付ファイルの有無に関係なく電子メールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントの設定手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

この記事の一部として、次の使用例について説明します。

1. ユーザーがTime Off Requestフォームに入力
1. フォーム送信時に、AEMワークフローがトリガーされます
1. AEMワークフローは、「電子メールを送信」コンポーネントを使用して、DoRを添付した電子メールを送信します

「電子メールの送信」手順を使用する前に、[configMgr](http://localhost:4502/system/console/configMgr)からDay CQ Mail Serviceを設定していることを確認します。 環境に固有の値を指定する

![Day CQ 電子メールサービスの設定](assets/mailservice.png)

この記事に関連付けられたアセットの一部として、次の情報が表示されます

1. 送信時にワークフローをトリガーするアダプティブフォーム
1. 添付ファイルとしてDORを含む電子メールを送信するサンプルワークフロー
1. メタデータプロパティを作成するOSGiバンドル

お使いのシステムでサンプルを実行するには、次の手順を実行してください。

1. [Developingwithserviceuserバンドルのデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)bundleのダウンロードとインストールこのバンドルには、ワークフローの処理手順の一部として、メタデータプロパティを作成するためのコードが含まれています。
1. [Day CQ 電子メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [パッケージマネージャーを使用してCRXにこの記事に関連付けられたアセットを読み込み、インストールします](assets/emaildoraemformskt.zip)
1. [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を起動します。 必須フィールドに入力し、送信します。
1. 添付ファイルとしてDocumentOfRecordを含む電子メールを受け取る必要があります

[ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)を調べます。

ワークフローのプロセスステップを確認します。 プロセス手順に関連付けられたカスタムコードは、メタデータプロパティ名を作成し、送信されたデータから値を設定します。これらの値は、送信電子メールコンポーネントで使用されます。

>[!NOTE]
>
>AEM Forms6.5以降では、メタデータプロパティを作成する際に、このカスタムコードは必要ありません。 AEMワークフローの変数機能を使用してください

電子メールの送信コンポーネントの「添付ファイル」タブが、以下のスクリーンショットに従って設定されていることを確認します
![「電子メール添付ファイルを送信」タブ](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」値は、アダプティブフォームの送信オプションで指定されたレコードパスのドキュメントで指定された値と一致する必要があります。

