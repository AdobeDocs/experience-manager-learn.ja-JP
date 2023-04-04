---
title: サンプルのデプロイ
description: ローカルのAEM Formsインスタンスで実行するユースケース
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6602
thumbnail: 6602.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: cdfae631-86d7-438f-9baf-afd621802723
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 1%

---

# サンプルのデプロイ

このユースケースをシステムで動作させるには、次の手順に従ってください。

>[!NOTE]
>ポート 4502 でAEM Formsを実行していると想定されます。


## データベースを作成

このサンプルでは、MySQL データベースを使用してアダプティブフォームのデータを保存します。 次を作成する必要があります： [スキーマファイルのインポートによるデータベーススキーマ](assets/data-base-schema.sql) を MySQL Workbench に追加します。

## データソースを作成

という名前のデータソースを作成する必要があります。 **StoreAndRetrieveAfData**. OSGi バンドル内のコードは、このデータソース名を使用します

## フォームデータモデルの作成

フォームデータモデルは、このデータソースに基づいて作成される必要があります。 **StoreAndRetrieveAfData**. このフォームデータモデルは、アプリケーション ID に関連付けられた携帯電話番号を取得するために使用されます。 フォームデータモデルは、 [ここからダウンロードしました。](assets/2-Factor-Authentication-DataSource-and-FDM.zip)

## Nexmo を使用した開発者アカウントの作成

で開発者アカウントを作成します。 [Nexmo](https://dashboard.nexmo.com/) OTP コードの送信と検証を行う。 API キーと API 秘密鍵をメモしておきます。 データソースとフォームデータモデルは、このサービスに対して既に作成されており、前の手順で説明したアセットに含まれています。

## 次の OSGi バンドルをデプロイします。

を持つバンドルをデプロイします。 [データベースのデータを保存して取得するためのコード](assets/FetchPartiallyCompletedForm.PartiallyCompletedForm.core-1.0-SNAPSHOT.jar)
をダウンロードして展開します。 [developingwithserviceuser.zip](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip).
Felix Web コンソールを使用して DevelopingWithServiceUser.jar ファイルをデプロイします。

## クライアントライブラリのデプロイ

このサンプルでは、2 つのクライアントライブラリを使用しています。 インポート [クライアントライブラリ](assets/client-libraries.zip) AEMに

## カスタムアダプティブフォームテンプレートの読み込み

このデモで使用されるサンプルフォームは、カスタムテンプレートに基づいています。 次をインポート： [AEMのカスタムテンプレート](assets/custom-template-with-page-component.zip)

## サンプルのアダプティブフォームを読み込む

このサンプルを構成する 2 つのフォームをAEMに読み込む必要があります。 サンプルフォームは、 [ここからダウンロード](assets/sample-forms.zip)

を開きます。 [MyAccountForm](http://localhost:4502/editor.html/content/forms/af/myaccountform.html) 編集モードで。 アダプティブフォーム内の適切なフィールドに、 API キーと API 秘密鍵の値を指定します。

## ソリューションをテストする

プレビュー [StoreAFWithAttachments](http://localhost:4502/content/dam/formsanddocuments/storeafwithattachments/jcr:content?wcmmode=disabled)
国コードを含む携帯電話番号を入力し、ユーザーの詳細を入力して添付ファイルを追加します。 「保存して終了」ボタンをクリックして、アダプティブフォームとその添付ファイルを保存します


## 使用例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)
