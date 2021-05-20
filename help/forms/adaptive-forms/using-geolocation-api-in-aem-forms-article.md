---
title: アダプティブFormsでのジオロケーションAPIの使用
seo-title: アダプティブFormsでのジオロケーションAPIの使用
description: 位置情報APIを使用して、フォーム上の住所フィールドを設定する
seo-description: 位置情報APIを使用して、フォーム上の住所フィールドを設定する
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: アダプティブフォーム
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 4%

---


# アダプティブForms{#using-geolocation-api-s-in-adaptive-forms}でのGeolocation APIの使用

この機能のライブデモへのリンクについては、[AEM Formsのサンプル](https://forms.enablementadobe.com/content/samples/samples.html?query=0)ページを参照してください。

この記事では、GoogleのジオロケーションAPIを使用してアダプティブフォームのフィールドにデータを入力する方法について説明します。 この使用例は、一般的に、フォーム上の現在の住所フィールドに値を入力する場合に使用します。

次の手順に従って、アダプティブFormsでGeolocation APIを使用しました。

1. [Googleマッププ](https://developers.google.com/maps/documentation/javascript/get-api-key) ラットフォームを使用するには、GoogleからAPIキーを取得します。1年間有効な試用版キーを入手できます。

1. アダプティブフォームフラグメントは、現在のアドレスを格納するフィールドを使用して作成されました

1. 位置情報APIは、アダプティブフォームの画像オブジェクトのclickイベントで呼び出されました。

1. API呼び出しによって返されたJSONデータが解析され、それに応じてアダプティブフォームフィールドの値が設定されました。

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![ジオロケーションapiで入力されるフィールド](assets/capture-4.gif)

1行目では、HTML Geolocation APIを使用して現在の場所を取得します。 現在の位置を取得したら、現在の位置をshowPosition関数に渡します。

showPosition関数では、Google APIを使用して、指定した緯度と経度のアドレスの詳細を取得します。

次に、APIから返されたJSONが解析され、アダプティブフォームのフィールドが設定されます。

>[!NOTE]
>
>テストの目的で、URLにlocalhostを含むHTTPプロトコルを使用できます。
>
>実稼動サーバーでこの機能を取得するには、AEM ServerのSSLを有効にする必要があります。
>
>この記事に関連するサンプルは、米国の住所でテストされています。 この機能を他の地理的な場所で使用する場合は、JSON解析を調整する必要が生じる場合があります。

この機能をサーバーに取り込むには、次の手順に従います

* AEM Formsサーバーをインストールして起動します。

>!![NOTE] この機能は、AEM Forms 6.3以降でテストされました。
* [Google APIキーの取得](https://developers.google.com/maps/documentation/javascript/get-api-key)を参照してください。
* [この記事に関連するアセットをAEMに読み込みます。](assets/geolocationapi.zip)
* [アダプティブフォームフラグメントを編集モードで開きます。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 画像選択コンポーネントのルールエディターを開きます。
* &lt;your_api_key>をGoogle APIキーに置き換えます。
* 変更を保存します。
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* 「位置情報」アイコンをクリックします。
* フォームには、現在の場所が入力されます。
