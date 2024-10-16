---
title: アダプティブフォームでの DAM 画像のインライン表示
description: アダプティブフォームでの DAM 画像のインライン表示
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
duration: 60
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '200'
ht-degree: 100%

---

# アダプティブフォームでの DAM 画像の表示

一般的なユースケースは、crx リポジトリにある画像をアダプティブフォーム内でインライン表示することです。

## プレースホルダー画像の追加

最初の手順では、プレースホルダー div をパネルコンポーネントの前に追加します。以下のコードでは、パネルコンポーネントは CSS クラス名 photo-upload で識別されます。JavaScript 関数は、アダプティブフォームに関連付けられているクライアントライブラリの一部です。この関数は、添付ファイルコンポーネントの initialize イベントで呼び出されます。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### インライン画像の表示

ユーザーが画像を選択すると、選択した画像名が非表示フィールド ImageName に入力されます。次に、この画像名が damURLToFile 関数に渡され、その関数が createFile 関数を呼び出して URL を Blob に変換し、FileReader.readAsDataURL() に渡します。

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### サーバーへのデプロイ

* AEM パッケージマネージャーを使用して、[クライアントライブラリとサンプル画像](assets/InlineDAMImage.zip)をダウンロードして AEM インスタンスにインストールします。
* AEM パッケージマネージャーを使用して、[サンプルフォーム](assets/FieldInspectionForm.zip)をダウンロードし、AEM インスタンスにインストールします。
* ブラウザーで [FielInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled) にアクセスします。
* フィクスチャーの 1 つを選択します。
* 画像がフォームに表示されます。
