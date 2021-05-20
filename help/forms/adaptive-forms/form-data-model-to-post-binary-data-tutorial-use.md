---
title: フォームデータモデルを使用したバイナリデータの投稿
seo-title: フォームデータモデルを使用したバイナリデータの投稿
description: フォームデータモデルを使用したAEM DAMへのバイナリデータの投稿
seo-description: フォームデータモデルを使用したAEM DAMへのバイナリデータの投稿
uuid: dd344ed8-69f7-4d63-888a-3c96993fe99d
feature: ワークフロー
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 6e99df7d-c030-416b-83d2-24247f673b33
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 1%

---


# フォームデータモデルを使用したバイナリデータの投稿&lt;A0/>{#using-form-data-model-to-post-binary-data}

AEM Forms 6.4以降では、フォームデータモデルサービスをAEMワークフローのステップとして呼び出す機能が追加されました。 この記事では、フォームデータモデルサービスを使用してレコードのドキュメントを投稿する場合の使用例について説明します。

使用例を次に示します。

1. ユーザーがアダプティブフォームに入力して送信します。
1. アダプティブフォームは、レコードのドキュメントを生成するように設定されます。
1. このアダプティブフォームの送信時に、AEM Workflowがトリガーされ、「フォームデータモデルを呼び出し」サービスを使用して、レコードのドキュメントをAEM DAMにPOSTします。

![posttodam](assets/posttodamshot1.png)

「フォームデータモデル」タブのプロパティ

「サービス入力」タブで、次のマッピングを行います

* ペイロードに対するDOR.pdfプロパティを含むファイル（保存する必要があるバイナリオブジェクト）。 つまり、アダプティブフォームが送信されると、生成されたレコードのドキュメントは、ワークフローペイロードに対するDOR.pdfというファイルに保存されます。**このDOR.pdfが、アダプティブフォームの送信プロパティを設定する際に指定したDOR.pdfと同じであることを確認します。**

* fileName - DAMにバイナリオブジェクトを保存する際の名前です。 したがって、このプロパティを動的に生成し、各fileNameが送信ごとに一意になるようにします。 この目的で、ワークフローのプロセスステップを使用して、filenameというメタデータプロパティを作成し、その値をフォームを送信する人のメンバー名とアカウント番号の組み合わせに設定しました。 例えば、ユーザーのメンバー名がJohn Jacobsで、アカウント番号が9846の場合、ファイル名はJohn Jacobs_9846.pdfになります。

![fdmserviceinput](assets/fdminputservice.png)

Service Input

>[!NOTE]
>
>トラブルシューティングのヒント — 何らかの理由でDOR.pdfがDAMに作成されていない場合は、[ここ](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)をクリックして、データソース認証設定をリセットします。 これらはAEM認証設定で、デフォルトはadmin/adminです。

ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

1.[Developingwithserviceuserバンドルをデプロイします](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。このカスタムOSGIバンドルは、メタデータプロパティを作成し、送信されたフォームデータから値を設定するために使用されます。

1. [この記事に関](assets/postdortodam.zip) 連するアセットを、パッケージマネージャーを使用してAEMにインポートします。次の情報が得られます

   1. ワークフローモデル
   1. AEM Workflowに送信するように設定されたアダプティブフォーム
   1. PostToDam.JSONファイルを使用するように設定されたデータソース
   1. データソースを使用するフォームデータモデル

1. [ブラウザーでアダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を開きます。
1. フォームに入力して送信します。
1. レコードのドキュメントが作成され、格納されている場合は、Assetsアプリケーションを確認します。


[データソ](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) ースの作成に使用するSwaggerファイルを参照できます
