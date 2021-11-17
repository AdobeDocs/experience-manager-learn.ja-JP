---
title: アダプティブFormsでのインライン画像の表示
description: アップロードした画像をアダプティブFormsにインラインで表示する
feature: Adaptive Forms
topics: development
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 0%

---

# アダプティブFormsのインライン画像

一般的な使用例としては、アップロードした画像をアダプティブフォーム内のインライン画像として表示する場合があります。 デフォルトでは、アップロードされた画像はリンクとして表示され、画像をアダプティブフォームに表示することで、このエクスペリエンスを強化できます。 この記事では、インライン画像の表示に関する手順について説明します。

## プレースホルダー画像を追加

最初の手順では、添付ファイルコンポーネントにプレースホルダー div を追加します。 以下のコードでは、添付ファイルコンポーネントは、photo-upload の CSS クラス名で識別されます。 JavaScript 関数は、アダプティブフォームに関連付けられているクライアントライブラリの一部です。 この関数は、添付ファイルコンポーネントの initialize イベントで呼び出されます。

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### インライン画像を表示

ユーザーが画像をアップロードすると、以下に示す関数が、ファイル添付コンポーネントの commit イベントで呼び出されます。 関数は、アップロードされたファイルオブジェクトを引数として受け取ります。

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### サーバーにデプロイ

* をダウンロードしてインストールする [クライアントライブラリ](assets/inline-image-client-library.zip) AEMパッケージマネージャーを使用してAEMインスタンス上で
* をダウンロードしてインストールする [サンプルフォーム](assets/inline-image-af.zip) AEMパッケージマネージャーを使用してAEMインスタンス上で
* ブラウザーで次の場所を指定します。 [インライン画像を追加](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* 「写真を添付」ボタンをクリックして画像を追加
