---
title: 電子メールを送信ステップのForms Workflow
seo-title: 電子メールを送信ステップのForms Workflow
description: AEM Forms 6.4で導入された電子メールの送信手順。この手順を使用して、添付ファイルの有無に関わらず電子メールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントの設定手順を説明します
seo-description: AEM Forms 6.4で導入された電子メールの送信手順。この手順を使用して、添付ファイルの有無に関わらず電子メールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントの設定手順を説明します
uuid: d054ebfb-3b9b-4ca4-8355-0eb0ee7febcb
feature: ワークフロー
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
discoiquuid: 3a11f602-2f4c-423a-baef-28824c0325a1
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '495'
ht-degree: 3%

---


# Forms Workflow{#using-send-email-step-of-forms-workflow}の電子メール送信手順の使用

AEM Forms 6.4で導入された電子メールの送信手順。この手順を使用して、添付ファイルの有無に関わらず電子メールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントの設定手順を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/21499/?quality=9&learn=on)

この記事の一部として、次の使用例について説明します。

1. ユーザーがオフリクエストフォームに入力する
1. フォームの送信時に、AEM Workflowがトリガーされます
1. AEMワークフローは、電子メールの送信コンポーネントを使用して、DoRを添付ファイルとして含む電子メールを送信します

「電子メールを送信」の手順を使用する前に、[configMgr](http://localhost:4502/system/console/configMgr)からDay CQ Mail Serviceを設定しておく必要があります。 環境に固有の値の指定

![Day CQ 電子メールサービスの設定](assets/mailservice.png)

この記事に関連付けられたアセットの一部として、次の情報が得られます

1. 送信時にワークフローをトリガーするアダプティブフォーム
1. DORを添付ファイルとしてEメールを送信するワークフローの例
1. メタデータプロパティを作成するOSGiバンドル

お使いのシステムでサンプルを実行するには、次の手順を実行します。

1. [Developingwithserviceuserバンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue bundleをダウンロードしてイ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)ンストールこのバンドルには、ワークフローのプロセスステップの一部としてメタデータプロパティを作成するためのコードが含まれます。
1. [Day CQ 電子メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [パッケージマネージャーを使用して、この記事に関連付けられたアセットをCRXに読み込み、インストールします](assets/emaildoraemformskt.zip)
1. [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を起動します。 必須フィールドに入力し、送信します。
1. DocumentOfRecordを添付ファイルとして含む電子メールが送信されます

[ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)を調べます。

ワークフローのプロセスステップを見てみましょう。 プロセスステップに関連付けられたカスタムコードは、メタデータのプロパティ名を作成し、送信されたデータから値を設定します。これらの値は、送信電子メールコンポーネントで使用されます。

>[!NOTE]
>
>AEM Forms 6.5以降では、メタデータプロパティを作成する際に、このカスタムコードは必要ありません。 AEM Workflowの変数機能を使用してください

電子メールを送信コンポーネントの「添付ファイル」タブが、以下のスクリーンショットに従って設定されていることを確認します。
![「電子メール添付ファイルを送信」タブ](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」の値は、アダプティブフォームの送信オプションで指定されたレコードのドキュメントパスで指定された値と一致する必要があります。

