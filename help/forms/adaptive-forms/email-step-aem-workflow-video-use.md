---
title: Forms Workflow のメール送信手順の使用
description: AEM Forms 6.4 で導入されたメール送信手順です。この手順を使用して、添付ファイルの有無に関わらずメールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、メール送信コンポーネントの設定手順について説明します。
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 21e58bbc-c1d6-4d41-a4d4-f522a3a5d4a7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 314
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '418'
ht-degree: 100%

---

# Forms Workflow のメール送信手順の使用 {#using-send-email-step-of-forms-workflow}

AEM Forms 6.4 で導入されたメール送信手順です。この手順を使用して、添付ファイルの有無に関わらずメールを送信できるビジネスプロセスやワークフローを構築できます。 次のビデオでは、メール送信コンポーネントの設定手順について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/21499?quality=12&learn=on)

この記事の中では、次の使用例について説明します。

1. ユーザーが休暇申請フォームに入力します
1. フォームの送信時に、AEM ワークフローがトリガーされます
1. AEM ワークフローは、メール送信コンポーネントを使用して、DoR が添付されたメールを送信します

メール送信手順を使用する前に、[configMgr](http://localhost:4502/system/console/configMgr) から Day CQ Mail Service を設定していることを確認してください。お使いの環境に合わせた値を指定します

![Day CQ メールサービスの設定](assets/mailservice.png)

この記事に関連付けられたアセットとして、次のものが得られます。

1. 送信時にワークフローをトリガーするアダプティブフォーム
1. DOR を添付ファイルとしてメールを送信するサンプルワークフロー
1. メタデータプロパティを作成する OSGi バンドル

お使いのシステムでサンプルを実行するには、次の手順を実行してください。

1. [Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue バンドルをダウンロードしてインストールします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)このバンドルには、ワークフローのプロセス手順の一部としてメタデータプロパティを作成するためのコードが含まれています。
1. [Day CQ メールサービスの設定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html?lang=ja)
1. [パッケージマネージャーを使用して、この記事に関連付けられたアセットを CRX に読み込んでインストールします。](assets/emaildoraemformskt.zip)
1. [アダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を起動します。必須フィールドに入力して送信します。
1. DocumentOfRecord を添付ファイルとして含むメールが届きます。

 [ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/emaildor.html)を探索

ワークフローのプロセス手順を見てみましょう。プロセス手順に関連付けられたカスタムコードは、メタデータのプロパティ名を作成し、送信されたデータから値を設定します。これらの値は、送信メールコンポーネントで使用されます。

>[!NOTE]
>
>AEM Forms 6.5 以降では、メタデータプロパティを作成するためにこのカスタムコードは必要ありません。AEM Workflow の変数機能を使用してください。

メール送信コンポーネントの「添付ファイル」タブが、以下のスクリーンショットに従って設定されていることを確認します。
![「送信メール添付ファイル」タブ](assets/sendemailcomponentconfigure.jpg)「DOR.pdf」の値は、アダプティブフォームの送信オプションで指定された、レコードパスのドキュメントで示された値と一致する必要があります。
