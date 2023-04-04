---
title: 電子メール送信ステップのForms Workflow
description: AEM Forms 6.4 で導入された電子メールの送信手順。この手順を使用して、添付ファイルの有無に関わらず電子メールを送信できるビジネスプロセスまたはワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントを設定する手順について説明します
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 3%

---

# 電子メール送信ステップのForms Workflow {#using-send-email-step-of-forms-workflow}

AEM Forms 6.4 で導入された電子メールの送信手順。この手順を使用して、添付ファイルの有無に関わらず電子メールを送信できるビジネスプロセスまたはワークフローを構築できます。 次のビデオでは、電子メール送信コンポーネントを設定する手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

この記事の一部として、次の使用例について説明します。

1. ユーザーがタイムオフリクエストフォームに入力します
1. フォームの送信時に、AEM Workflow がトリガーされます
1. AEMワークフローは、電子メールの送信コンポーネントを使用して、DoR を添付ファイルとして含む電子メールを送信します

電子メールの送信手順を使用する前に、Day CQ Mail Service を [configMgr](http://localhost:4502/system/console/configMgr). 環境に固有の値を指定する

![Day CQ メールサービスの設定](assets/mailservice.png)

この記事に関連するアセットの一部として、次の情報が得られます

1. 送信時にワークフローをトリガーするアダプティブフォーム
1. DOR を添付ファイルとして E メールを送信するサンプルワークフロー
1. メタデータプロパティを作成する OSGi バンドル

お使いのシステムでサンプルを実行するには、次の手順を実行してください。

1. [Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue バンドルをダウンロードしてインストールする](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)このバンドルには、ワークフローのプロセスステップの一部としてメタデータプロパティを作成するためのコードが含まれています。
1. [Day CQ メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html)
1. [パッケージマネージャーを使用して、この記事に関連するアセットを CRX に読み込んでインストールします](assets/emaildoraemformskt.zip)
1. を起動します。 [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled). 必須フィールドに入力し、送信します。
1. DocumentOfRecord を添付ファイルとして含む電子メールが届きます

関連トピック [ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)

ワークフローのプロセスステップを見てみましょう。 プロセスステップに関連付けられたカスタムコードは、メタデータのプロパティ名を作成し、送信されたデータから値を設定します。これらの値は、送信電子メールコンポーネントで使用されます。

>[!NOTE]
>
>AEM Forms 6.5 以降では、メタデータプロパティを作成するためにこのカスタムコードは必要ありません。 AEM Workflow の変数機能を使用してください

電子メールを送信コンポーネントの「添付ファイル」タブが、以下のスクリーンショットに従って設定されていることを確認します。
![「E メール添付ファイルを送信」タブ](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」の値は、アダプティブフォームの送信オプションで指定されたレコードのドキュメントのパスで指定された値と一致する必要があります。
