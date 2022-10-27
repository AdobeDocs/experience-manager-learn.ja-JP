---
title: アダプティブFormsでのインライン画像の表示
description: アップロードした画像をアダプティブFormsにインラインで表示する
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
source-git-commit: e1c16ff347f5f398c7bc47233049427eeffa2aab
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# アダプティブFormsでの DAM 画像の表示

一般的な使用例は、crx リポジトリ内の画像をアダプティブフォーム内でインラインで表示する場合です。

## プレースホルダー画像を追加

最初の手順では、プレースホルダー div をパネルコンポーネントの前に追加します。 パネルコンポーネントの下のコードでは、photo-upload の CSS クラス名で識別されます。 JavaScript 関数は、アダプティブフォームに関連付けられているクライアントライブラリの一部です。 この関数は、添付ファイルコンポーネントの initialize イベントで呼び出されます。

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### インライン画像を表示

ユーザーが画像を選択すると、非表示フィールドの ImageName に、選択した画像名が入力されます。 次に、この画像名が damURLToFile 関数に渡され、createFile 関数を呼び出して FileReader.readAsDataURL() の URL を Blob に変換します。

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

### サーバーにデプロイ

* をダウンロードしてインストールする [クライアントライブラリとサンプル画像](assets/InlineDAMImage.zip) AEM Package Manager を使用してAEMインスタンス上で
* をダウンロードしてインストールする [サンプルフォーム](assets/FieldInspectionForm.zip) AEMパッケージマネージャーを使用してAEMインスタンス上で
* ブラウザーで次の場所を指定します。 [FielInspectionForm](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* 器具の 1 つを選択
* 画像がフォームに表示されます