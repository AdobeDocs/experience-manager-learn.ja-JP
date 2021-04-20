---
title: サンプルのデプロイ
description: ローカルのAEM Formsインスタンスで実行するユースケースを取得する
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: 開発
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---



# サンプルのデプロイ

ご使用のシステムでこの使用事例を動作させるには、次の手順に従ってください。

>[!NOTE]
>ポート4502でAEM Formsを実行していると想定します。


## データベースの作成

このサンプルでは、MySQLデータベースを使用してアダプティブフォームデータを保存しています。 スキーマファイル](assets/data-base-schema.sql)をMySQL Workbenchにインポートして、[データベーススキーマを作成する必要があります。

## データソースの作成

**StoreAndRetrieveAfData**&#x200B;という名前のデータソースを作成する必要があります。 OSGiバンドル内のコードは、このデータソース名を使用します

## フォームデータモデルを作成

**StoreAndRetrieveAfData**&#x200B;というこのデータソースに基づいて、フォームデータモデルを作成する必要があります。 このフォームデータモデルは、アプリケーションIDに関連付けられた携帯電話番号を取得するために使用されます。 フォームデータモデルは、[ここからダウンロードできます。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## nexmoを使用した開発者アカウントの作成

OTPコードを送信および確認するために、[Nexmo](https://dashboard.nexmo.com/)で開発者アカウントを作成します。 APIキーとAPI秘密鍵をメモしておきます。 データソースとフォームのデータモデルは、このサービスに対して既に作成済みで、前の手順で説明したアセットに含まれています。

## 次のOSGiバンドルのデプロイ

データベース](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)からデータを取得する[コードを持つバンドルを展開します
[DevelopingWithServiceUser Bundle](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)を展開します。

## クライアントライブラリのデプロイ

このサンプルでは、2つのクライアントライブラリを使用しています。 これらの[クライアントライブラリ](assets/client-libraries.zip)をAEMに読み込みます。

## カスタムアダプティブフォームテンプレートの読み込み

このデモで使用されるサンプルフォームは、カスタムテンプレートに基づいています。 [カスタムテンプレートをAEM](assets/custom-template-with-page-component.zip)に読み込みます

## サンプルのアダプティブフォームの読み込み

このサンプルを構成する2つのフォームをAEMに読み込む必要があります。 サンプルフォームは、[ここから](assets/sample-forms.zip)ダウンロードできます。

[MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html)を編集モードで開きます。 アダプティブフォームの該当するフィールドにAPIキーとAPIシークレットの値を指定します。

## ソリューションのテスト

[StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)のプレビュー
国コードを含むモバイル番号を入力し、ユーザーの詳細を入力して添付ファイルを追加します。 「保存して終了」ボタンをクリックして、アダプティブフォームとその添付ファイルを保存します


## 使用事例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
