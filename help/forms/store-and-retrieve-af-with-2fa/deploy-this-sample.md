---
title: サンプルのデプロイ
description: ローカルのAEM Formsインスタンスで実行するユースケース
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 1%

---



# サンプルのデプロイ

この使用例をシステムで動作させるには、次の手順に従ってください。

>[!NOTE]
>ポート4502でAEM Formsを実行していると想定されます。


## データベースの作成

このサンプルでは、MySQLデータベースを使用してアダプティブフォームデータを保存します。 スキーマファイル](assets/data-base-schema.sql)をMySQL Workbenchに読み込んで、[データベーススキーマを作成する必要があります。

## データソースの作成

**StoreAndRetrieveAfData**&#x200B;というデータソースを作成する必要があります。 OSGiバンドル内のコードは、このデータソース名を使用します

## フォームデータモデルを作成

フォームデータモデルは、**StoreAndRetrieveAfData**&#x200B;というこのデータソースに基づいて作成する必要があります。 このフォームデータモデルは、アプリケーションIDに関連付けられた携帯電話番号を取得するために使用されます。 フォームデータモデルは、[ここからダウンロードできます。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Nexmoを使用した開発者アカウントの作成

OTPコードを送信および検証するための[Nexmo](https://dashboard.nexmo.com/)の開発者アカウントを作成します。 APIキーとAPI秘密鍵をメモしておきます。 データソースとフォームのデータモデルは、このサービスに対して既に作成されており、前の手順で説明したアセットに含まれています。

## 次のOSGiバンドルをデプロイします。

データベース](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)からデータを保存および取得する[コードを含むバンドルをデプロイします。
[DevelopingWithServiceUserバンドル](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)をデプロイします。

## クライアントライブラリのデプロイ

このサンプルでは、2つのクライアントライブラリを使用しています。 これらの[クライアントライブラリ](assets/client-libraries.zip)をAEMに読み込みます。

## カスタムアダプティブフォームテンプレートの読み込み

このデモで使用されるサンプルフォームは、カスタムテンプレートに基づいています。 [カスタムテンプレートをAEM](assets/custom-template-with-page-component.zip)に読み込みます。

## サンプルのアダプティブフォームを読み込む

このサンプルを構成する2つのフォームをAEMに読み込む必要があります。 サンプルフォームは、[こちら](assets/sample-forms.zip)からダウンロードできます。

[MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html)を編集モードで開きます。 アダプティブフォーム内の該当するフィールドに、APIキーとAPI秘密鍵の値を指定します。

## ソリューションのテスト

[StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)をプレビューします。
国コードを含む携帯電話番号を入力し、ユーザーの詳細を入力して添付ファイルを追加します。 「保存して終了」ボタンをクリックして、アダプティブフォームとその添付ファイルを保存します


## 使用例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)
