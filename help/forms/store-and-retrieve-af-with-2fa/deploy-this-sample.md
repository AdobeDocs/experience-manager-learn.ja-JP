---
title: サンプルのデプロイ
description: ローカルのAEM Formsインスタンスで実行するユースケースを取得する
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 1%

---



# サンプルのデプロイ

ご使用のシステムでこの使用事例を動作させるには、次の手順に従ってください。

>[!NOTE]
>ポート4502でAEM Formsを実行していると想定します。


## データベースの作成

このサンプルでは、MySQLデータベースを使用してアダプティブフォームデータを保存しています。 スキーマファイルをMySQL Workbenchに読み込んで、 [データベーススキーマを作成する必要があります](assets/data-base-schema.sql) 。

## データソースの作成

StoreAndRetrieveAfDataという名前のデータソースを作成する必要があり **ます**。 OSGiバンドル内のコードは、このデータソース名を使用します

## フォームデータモデルを作成

StoreAndRetrieveAfDataというこのデータソースに基づいてフォームデータモデルを作成する必要があり **ます**。 このフォームデータモデルは、アプリケーションIDに関連付けられた携帯電話番号を取得するために使用されます。 フォームデータモデルは、ここから [ダウンロードできます。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## nexmoを使用した開発者アカウントの作成

OTPコードを送信および確認するための [Nexmo](https://dashboard.nexmo.com/) Developerアカウントを作成します。 APIキーとAPI秘密鍵をメモしておきます。 データソースとフォームのデータモデルは、このサービスに対して既に作成済みで、前の手順で説明したアセットに含まれています。

## 次のOSGiバンドルのデプロイ

データベースにデータを格納および取得する [コードを持つバンドルを展開します。DevelopingWithServiceUserバンドルを](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)展開します [](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)。

## クライアントライブラリのデプロイ

このサンプルでは、2つのクライアントライブラリを使用しています。 これらの [クライアントライブラリをAEMに読み込みます](assets/client-libraries.zip) 。

## カスタムアダプティブフォームテンプレートの読み込み

このデモで使用されるサンプルフォームは、カスタムテンプレートに基づいています。 AEMへの [カスタムテンプレートの読み込み](assets/custom-template-with-page-component.zip)

## サンプルのアダプティブフォームの読み込み

このサンプルを構成する2つのフォームをAEMに読み込む必要があります。 サンプルフォームはここから [ダウンロードできます。](assets/sample-forms.zip)

Open the [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) in edit mode. アダプティブフォームの該当するフィールドにAPIキーとAPIシークレットの値を指定します。

## ソリューションのテスト

StoreAFWithAttachmentsのプレビュー国コードを含むモバイル [番号を](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)入力し、ユーザーの詳細を入力して添付ファイルを追加します。 「保存して終了」ボタンをクリックして、アダプティブフォームとその添付ファイルを保存します


## 使用事例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
