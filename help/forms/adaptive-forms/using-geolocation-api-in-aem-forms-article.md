---
title: アダプティブフォームでの Geolocation API の使用
description: Geolocation API を使用して、フォーム上の住所フィールドに入力します
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
duration: 88
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '366'
ht-degree: 100%

---

# アダプティブフォームでの Geolocation API の使用{#using-geolocation-api-s-in-adaptive-forms}

この記事では、Google の Geolocation API を使用してアダプティブフォームのフィールドに値を入力する方法について説明します。 このユースケースは、一般的に、フォーム上の現在のアドレスフィールドに値を入力する場合に使用します。

次の手順に従って、アダプティブフォームで Geolocation API を使用しました。

1. [Google から API キー](https://developers.google.com/maps/documentation/javascript/get-api-key)を取得して、Google マッププラットフォームを使用します。1 年間有効なトライアルキーを入手できます。

1. 現在のアドレスを格納するフィールドを持つアダプティブフォームフラグメントが作成されました

1. Geolocation API は、アダプティブフォームの画像オブジェクトのクリックイベントで呼び出されました。

1. API 呼び出しによって返された JSON データは解析され、アダプティブフォームのフィールドの値は適切に設定されました。

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

![Geolocation API でフィールドに入力する](assets/capture-4.gif)

行 1 では、現在の位置を取得するために HTML Geolocation API を使用します。 現在の位置を取得したら、現在の位置を showPosition 関数に渡します。

showPosition 関数では、Google API を使用して、指定した緯度と経度のアドレスの詳細を取得します。

次に、API から返された JSON が解析され、アダプティブフォームのフィールドが設定されます。

>[!NOTE]
>
>テストの目的で、URL で localhost を含む HTTP プロトコルを使用できます。
>
>実稼動サーバーでこの機能を取得するには、AEM サーバーの SSL を有効にする必要があります。
>
>この記事に関連するサンプルは、米国の住所でテストされています。 この機能を他の地理的な場所で使用する場合は、JSON 解析の調整が必要になる場合があります。

この機能をサーバーに取り込むには、次の手順に従ってください

* AEM Forms サーバーをインストールして起動します。
> この機能は、AEM Forms 6.3 以降でテストされました。
* [Google API キーを取得します](https://developers.google.com/maps/documentation/javascript/get-api-key)。
* [この記事に関連するアセットを AEM に読み込みます。](assets/geolocationapi.zip)
* [アダプティブフォームフラグメントをオーサリングモードで開きます。](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* 画像選択コンポーネントのルールエディターを開きます。
* &lt;your_api_key> をGoogle API キーに置き換えます。
* 変更を保存します。
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* 「位置情報」アイコンをクリックします。
* フォームには、現在の場所を入力する必要があります。
