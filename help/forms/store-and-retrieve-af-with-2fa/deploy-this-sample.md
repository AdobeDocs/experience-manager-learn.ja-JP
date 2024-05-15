---
title: サンプルのデプロイ
description: ローカルの AEM Forms インスタンスで動作しているユースケースの取得
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
duration: 186
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 100%

---

# サンプルのデプロイ

このユースケースをシステムで動作させるには、次の手順に従ってください。

>[!NOTE]
>ポート 4502 で AEM Forms を実行していると想定します。


## データベースの作成

このサンプルでは、MySQL データベースを使用してアダプティブフォームのデータを保存しています。 [スキーマファイルを MySQL Workbench に読み込んで、データベーススキーマ](assets/data-base-schema.sql)を作成する必要があります。

## データソースの作成

前の手順で作成したデータベーススキーマを指す **StoreAndRetrieveAfData** という Apache Sling Connection Pooled DataSource を作成する必要があります。OSGi バンドル内のコードでは、このデータソース名を使用します。

## フォームデータモデルの作成

**StoreAndRetrieveAfData** と呼ばれるこのデータソースに基づいて、フォームデータモデルを作成する必要があります。このフォームデータモデルを使用して、アプリケーション ID に関連付けられた携帯電話番号を取得します。フォームデータモデルは[こちらからダウンロード](assets/2-Factor-Authentication-DataSource-and-FDM.zip)できます。

## Nexmo を使用した開発者アカウントの作成

[Nexmo](https://dashboard.nexmo.com/) で開発者アカウントを作成し、OTP コードの送信と検証を行います。 API キーと API 秘密鍵をメモしておきます。 データソースとフォームデータモデルは、このサービスに対して既に作成されており、前の手順で説明したアセットに含まれています。

## 次の OSGi バンドルのデプロイ

[データベースに対してデータの保存と取得を行うためのコード](assets/SaveAndResume.core-1.0.0-SNAPSHOT.jar)を含んだバンドルをデプロイします。
[developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip) をダウンロードして解凍します。
Felix web コンソールを使用して DevelopingWithServiceUser.jar ファイルをデプロイします。

## クライアントライブラリのデプロイ

このサンプルでは、2 つのクライアントライブラリを使用します。 これらの[クライアントライブラリ](assets/store-af-with-attachments-client-lib.zip)を AEM に読み込みます。

## カスタムアダプティブフォームテンプレートの読み込み

このデモで使用するサンプルフォームは、カスタムテンプレートに基づいています。 [AEM へのカスタムテンプレート](assets/custom-template-with-page-component.zip)の読み込み

## サンプルのアダプティブフォームの読み込み

このサンプルを構成する 2 つのフォームを AEM に読み込む必要があります。 サンプルフォームは、[こちらからダウンロード](assets/sample-forms.zip)できます。

[MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) を編集モードで開きます。アダプティブフォームの適切なフィールドに、Vonage API キーおよび API 秘密鍵の値を指定します。

## ソリューションのテスト

[StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled) をプレビューします。
国コードを含んだ携帯電話番号を入力し、ユーザーの詳細を入力して添付ファイルを追加します。「保存して終了」ボタンをクリックして、アダプティブフォームとその添付ファイルを保存します。


## ユースケースのデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
