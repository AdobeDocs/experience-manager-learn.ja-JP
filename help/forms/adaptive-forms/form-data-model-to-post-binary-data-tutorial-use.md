---
title: フォームデータモデルを使用したバイナリデータの投稿
description: フォームデータモデルを使用したAEM DAM へのバイナリデータの投稿
feature: Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 9c62a7d6-8846-424c-97b8-2e6e3c1501ec
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 0%

---

# フォームデータモデルを使用したバイナリデータの投稿{#using-form-data-model-to-post-binary-data}

AEM Forms 6.4 以降では、AEM Workflow のステップとしてフォームデータモデルサービスを呼び出す機能が追加されました。 この記事では、フォームデータモデルサービスを使用してレコードのドキュメントを投稿する場合のサンプルの使用例について説明します。

使用例を次に示します。

1. ユーザーがアダプティブフォームに入力して送信します。
1. アダプティブフォームは、レコードのドキュメントを生成するように設定されます。
1. このアダプティブフォームの送信時に、AEM Workflow がトリガーされ、「フォームデータモデルサービスを起動」を使用してレコードのドキュメントをAEM DAM にPOSTします。

![posttodam](assets/posttodamshot1.png)

「フォームデータモデル」タブのプロパティ

「サービス入力」タブで、以下をマッピングします

* ペイロードを基準とした DOR.pdf プロパティを持つファイル（保存する必要があるバイナリオブジェクト）。 つまり、アダプティブフォームが送信されると、生成されたレコードのドキュメントは、ワークフローのペイロードを基準とした DOR.pdf と呼ばれるファイルに保存されます。**この DOR.pdf が、アダプティブフォームの送信プロパティを設定する際に指定した DOR.pdf と同じであることを確認します。**

* fileName - DAM にバイナリオブジェクトを保存する際の名前です。 したがって、このプロパティを動的に生成し、各 fileName が送信ごとに一意になるようにします。 この目的で、ワークフローのプロセスステップを使用して、filename というメタデータプロパティを作成し、その値をフォーム送信者のメンバー名とアカウント番号の組み合わせに設定しました。 例えば、ユーザーのメンバー名が John Jacobs で、そのアカウント番号が 9846 の場合、ファイル名は John Jacobs_9846.pdf になります。

![fdmserviceinput](assets/fdminputservice.png)

Service Input

>[!NOTE]
>
>トラブルシューティングのヒント — 何らかの理由で DOR.pdf が DAM に作成されていない場合は、「 [ここ](http://localhost:4502/mnt/overlay/fd/fdm/gui/components/admin/fdmcloudservice/properties.html?item=%2Fconf%2Fglobal%2Fsettings%2Fcloudconfigs%2Ffdm%2Fpostdortodam). これらはAEM認証設定で、デフォルトは admin/admin です。

お使いのサーバーでこの機能をテストするには、次の手順に従ってください。

1.[Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

1. [setvalue バンドルをダウンロードしてデプロイします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar).このカスタム OSGi バンドルは、メタデータプロパティの作成と、送信されたフォームデータからの値の設定に使用されます。

1. [アセットの読み込み](assets/postdortodam.zip) パッケージマネージャーを使用してこの記事に関連付けられてAEMになります。次の情報が得られます

   1. ワークフローモデル
   1. AEM Workflow に送信するように設定されたアダプティブフォーム
   1. PostToDam.JSON ファイルを使用するように設定されたデータソース
   1. データソースを使用するフォームデータモデル

1. ポイント： [アダプティブフォームを開くブラウザー](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
1. フォームに入力して送信します。
1. レコードのドキュメントが作成され、保存されている場合は、Assets アプリケーションを確認します。


[Swagger ファイル](http://localhost:4502/conf/global/settings/cloudconfigs/fdm/postdortodam/jcr:content/swaggerFile) データソースの作成に使用されている場合は、参照用に使用できます
