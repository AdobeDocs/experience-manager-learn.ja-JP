---
title: アダプティブFormsでのインライン画像の表示
seo-title: アダプティブFormsでのインライン画像の表示
description: アップロードされた画像をアダプティブFormsにインラインで表示
seo-description: アップロードされた画像をアダプティブFormsにインラインで表示
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 1%

---


# アダプティブFormsのインライン画像

一般的な使用例として、アップロードした画像をアダプティブフォーム内のインライン画像として表示します。 デフォルトでは、アップロードされた画像はリンクとして表示され、画像をアダプティブフォームに表示することで、このエクスペリエンスを強化できます。 この記事では、インライン画像の表示に関する手順について説明します。

## プレースホルダー画像を追加

最初の手順は、添付ファイルコンポーネントにプレースホルダーdivを追加することです。 添付ファイルコンポーネントの下のコードでは、photo-uploadのCSSクラス名で識別されます。 JavaScript関数は、アダプティブフォームに関連付けられたクライアントライブラリの一部です。 この関数は、添付ファイルコンポーネントのinitializeイベントで呼び出されます。

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

ユーザーが画像をアップロードした後、添付ファイルコンポーネントのcommitイベントで以下に示す関数が呼び出されます。 関数は、アップロードされたファイルオブジェクトを引数として受け取ります。

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

### サーバーにデプロイする

* AEMパッケージマネージャーを使用して、AEMインスタンスに[クライアントライブラリ](assets/inline-image-client-library.zip)をダウンロードし、インストールします。
* AEMパッケージマネージャーを使用して、AEMインスタンスに[サンプルフォーム](assets/inline-image-af.zip)をダウンロードし、インストールします。
* ブラウザーで[インライン画像を追加](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)を参照します。
* 「写真を添付」ボタンをクリックして画像を追加します