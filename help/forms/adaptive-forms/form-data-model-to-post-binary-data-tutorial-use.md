---
title: フォームデータモデルを使用したバイナリデータの投稿
description: フォームデータモデルを使用した AEM DAM へのバイナリデータの投稿
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
last-substantial-update: 2021-01-09T00:00:00Z
duration: 95
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 100%

---

# フォームデータモデルを使用したバイナリデータの投稿{#using-form-data-model-to-post-binary-data}

AEM Forms 6.4 以降、AEM ワークフローのステップとしてフォームデータモデルサービスを呼び出すことができるようになりました。この記事では、フォームデータモデルサービスを使用してレコードのドキュメントを投稿する場合のサンプルのユースケースについて説明します

ユースケースを次に示します。

1. ユーザーがアダプティブフォームに入力して送信します。
1. アダプティブフォームは、レコードのドキュメントを生成するように設定されています。
1. このアダプティブフォームの送信時に、AEM ワークフローがトリガーされ、フォームデータモデルサービスの呼び出しを使用して、レコードのドキュメントを AEM DAM に投稿します。

![posttodam](assets/posttodamshot1.png)

「フォームデータモデル」タブ - プロパティ

「サービス入力」タブで、以下をマッピングします

* ペイロードに関連する DOR.pdf プロパティを持つファイル（保存する必要があるバイナリオブジェクト）。つまり、アダプティブフォームが送信されると、生成されたレコードのドキュメントが、ワークフローペイロードに関連する DOR.pdf と呼ばれるファイルに保存されます。**この DOR.pdf が、アダプティブフォームの送信プロパティを設定する際に指定した DOR.pdf と同じであることを確認します。**

* fileName - DAM にバイナリオブジェクトを保存する際の名前です。したがって、各 fileName が送信ごとに一意になるように、このプロパティを動的に生成する必要があります。この目的のために、ワークフローのプロセスステップを使用して filename と呼ばれるメタデータプロパティを作成し、その値をフォームを送信するユーザーのメンバー名とアカウント番号の組み合わせに設定しました。例えば、そのユーザーのメンバー名が John Jacobs で、アカウント番号が 9846 の場合、ファイル名は John Jacobs_9846.pdf になります

![fdmserviceinput](assets/fdminputservice.png)

サービス入力

>[!NOTE]
>
>トラブルシューティングのヒント - 何らかの理由で DOR.pdf が DAM に作成されない場合は、[こちら](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam)をクリックして、データソース認証設定をリセットします。これらは AEM 認証設定で、デフォルトでは admin/admin です。

サーバーでこの機能をテストするには、以下の手順に従ってください。

1.[Developingwithserviceuser バンドルをデプロイします](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue バンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。このカスタム OSGi バンドルは、メタデータプロパティの作成と、送信されたフォームデータからのその値の設定に使用されます。

1. パッケージマネージャーを使用して、AEM にこの記事に関連する[アセットを読み込み](assets/postdortodam.zip)ます。次のようになります。

   1. ワークフローモデル
   1. AEM ワークフローに送信するように設定されたアダプティブフォーム
   1. PostToDam.JSON ファイルを使用するように設定されたデータソース
   1. データソースを使用するフォームデータモデル

1. [ブラウザーでアダプティブフォームを開く](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)ように指定します
1. フォームに入力して送信します。
1. レコードのドキュメントが作成されて保存されているかどうか、アセットアプリケーションを確認します。


データソースの作成に使用する [Swagger ファイル](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile)を参照用に入手できます
