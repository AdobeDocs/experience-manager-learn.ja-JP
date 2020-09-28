---
title: アダプティブFormsでのGeolocation APIの使用
seo-title: アダプティブFormsでのGeolocation APIの使用
description: 位置情報APIの
seo-description: 位置情報APIの
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: adaptive-forms,
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 4%

---


# アダプティブFormsでのGeolocation APIの使用{#using-geolocation-api-s-in-adaptive-forms}

この機能のライブデモへのリンクは、 [AEM Formsのサンプルページをご覧ください](https://forms.enablementadobe.com/content/samples/samples.html?query=0) 。

この記事では、GoogleのGeolocation APIを使用してアダプティブフォームのフィールドに入力する方法を見ていきます。 この使用例は、フォーム上の現在の住所フィールドに値を入力する場合に一般的に使用されます。

次の手順に従って、アダプティブFormsのGeolocation APIを使用しました。

1. [Google Mapsプラットフォームを使用するには](https://developers.google.com/maps/documentation/javascript/get-api-key) 、APIキーをGoogleから取得します。 1年間有効な試用版キーを取得できます。

1. 現在の住所を保持するフィールドを持つアダプティブフォームフラグメントが作成されました

1. Geolocation APIは、アダプティブフォームの画像オブジェクトのクリックイベント時に呼び出されました。

1. API呼び出しによって返されたJSONデータが解析され、それに応じてアダプティブフォームのフィールド値が設定されました。

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

![GeoLocation APIで入力されるフィールド](assets/capture-4.gif)

1行目では、現在の場所を取得するためにHTML Geolocation APIを使用します。 現在の位置が取得されたら、現在の位置をshowPosition関数に渡します。

showPosition関数では、Google APIを使用して、指定した緯度と経度に関する住所の詳細を取得します。

次に、APIから返されたJSONが解析され、アダプティブフォームのフィールドが設定されます。

>[!NOTE]
>
>テスト用に、URLにlocalhostを含むHTTPプロトコルを使用できます。
>
>実稼働サーバーでこの機能を使用するには、AEMサーバーでSSLを有効にする必要があります。
>
>この記事に関連付けられているサンプルは、米国の住所でテスト済みです。 この機能を他の地域で使用する場合は、JSON解析を調整する必要があります。

この機能をサーバーに接続するには、次の手順に従ってください

* AEM Forms・サーバをインストールして開始します。

>!![NOTE] この機能は、AEM Forms6.3以降でテストされました。
* [Google APIキーの取得](https://developers.google.com/maps/documentation/javascript/get-api-key)。
* [この記事に関連するアセットをAEMに読み込みます。](assets/geolocationapi.zip)
* [アダプティブフォームフラグメントを編集モードで開きます。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 画像選択コンポーネントのルールエディターを開きます。
* &lt;your_api_key>をGoogle APIキーに置き換えます。
* 変更を保存します。
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* 「位置情報」アイコンをクリックします。
* フォームに現在の場所が入力されます。
