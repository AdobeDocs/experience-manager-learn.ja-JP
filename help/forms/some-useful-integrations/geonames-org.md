---
title: カスケードドロップダウンリスト
description: 以前のドロップダウンリスト選択に基づいてドロップダウンリストを生成します。
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# カスケードドロップダウンリスト

カスケードコンボボックスは、1 つの DropDownList コントロールが親または以前の DropDownList コントロールに依存する、依存する DropDownList コントロールの一連です。 DropDownList コントロールの項目は、別の DropDownList コントロールからユーザーが選択した項目に基づいて設定されます。

## 使用例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=9&learn=on)

このチュートリアルでは、 [Geonames REST API](http://api.geonames.org/) を使用して、この機能を試すことができます。
この種のサービスを提供する組織は多数あり、REST API が詳細に文書化されている限り、データ統合機能を使用してAEM Formsと簡単に統合できます

次の手順に従って、AEM Formsにカスケードドロップダウンリストを実装しました

## 開発者アカウントを作成

で開発者アカウントを作成します。 [地域名](https://www.geonames.org/login). ユーザー名をメモします。 このユーザー名は、geonames.org の REST API を呼び出すために必要です。

## Swagger/OpenAPI ファイルの作成

OpenAPI Specification（旧称 Swagger Specification）は、REST API の API 記述形式です。 OpenAPI ファイルを使用すると、次のような API 全体を記述できます。

* 使用可能なエンドポイント (/users) および各エンドポイントでの操作 (GET/users、POST/users)
* 操作パラメータ各操作の入力と出力認証方法
* 連絡先情報、ライセンス、利用条件、その他の情報。
* API の仕様は、YAML または JSON で記述できます。 フォーマットは、学習が容易で、人間と機械の両方に対して読み取り可能です。

最初の Swagger/OpenAPI ファイルを作成するには、 [OpenAPI ドキュメント](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> AEM Formsは、OpenAPI Specification バージョン 2.0(FKA Swagger) をサポートしています。

以下を使用： [swagger editor](https://editor.swagger.io/) を使用して、国または州のすべての国と子要素を取得する操作を記述する swagger ファイルを作成します。 Swagger ファイルは、JSON 形式または YAML 形式で作成できます。 完成した Swagger ファイルは、 [ここ](assets/swagger-files.zip)
Swagger ファイルは、次の REST API を記述します
* [すべての国を取得](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Geoname オブジェクトの子を取得](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## データソースの作成

AEM/AEM Formsをサードパーティのアプリケーションと統合するには、 [データソースを作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) クラウドサービス設定の「 」。 以下を使用してください： [swagger ファイル](assets/swagger-files.zip) をクリックして、データソースを作成します。
2 つのデータソースを作成する必要があります（1 つはすべての国を取得するためのデータソースで、他のものは子要素を取得するためのデータソースです）


## フォームデータモデルの作成

AEM Formsのデータ統合は、を作成して操作するための直感的なユーザーインターフェイスを提供します [フォームデータモデル](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). 前の手順で作成したデータソースに基づいて、フォームデータモデルを作成します。 2 つのデータソースを持つフォームデータモデル

![fdm](assets/geonames-fdm.png)


## アダプティブフォームを作成

フォームGETモデルのデータ呼び出しをアダプティブフォームに統合して、ドロップダウンリストに入力します。
2 つのドロップダウンリストを持つアダプティブフォームを作成します。 1 つは国をリストするためのもので、もう 1 つは選択した国に応じて州や都道府県をリストするものです。

### 国ドロップダウンリストの入力

国リストは、フォームが最初に初期化されたときに入力されます。 次のスクリーンショットは、国ドロップダウンリストのオプションを入力するように設定されたルールエディターを示しています。 この機能を使用するには、ジオネームアカウントをユーザー名に指定する必要があります。
![get-countries](assets/get-countries-rule-editor.png)

#### 都道府県ドロップダウンリストの入力

選択した国に基づいて、都道府県ドロップダウンリストを生成する必要があります。 次のスクリーンショットに、ルールエディターの設定を示します。
![state-province-options](assets/state-province-options.png)

### 演習

選択した国と都道府県に基づいて郡と市区町村をリストするには、フォームに郡と市区町村と呼ばれる 2 つのドロップダウンリストを追加します。
![運動](assets/cascading-drop-down-exercise.png)
