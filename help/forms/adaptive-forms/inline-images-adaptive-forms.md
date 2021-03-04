---
title: アダプティブFormsでのインライン画像の表示
seo-title: アダプティブFormsでのインライン画像の表示
description: アップロードされた画像をアダプティブFormsでインラインで表示
seo-description: アップロードされた画像をアダプティブFormsでインラインで表示
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 1%

---


# アダプティブFormsのインライン画像

一般的な使用例は、アップロードした画像をアダプティブフォーム内でインライン画像として表示する場合です。 デフォルトでは、アップロードされた画像はリンクとして表示され、アダプティブフォームで画像を表示することで、このエクスペリエンスを強化できます。 この記事では、インライン画像の表示に関する手順について説明します。

## プ追加レースホルダー画像

最初の手順は、プレースホルダーdivを添付ファイルコンポーネントの前に追加することです。 以下のコードでは、添付ファイルコンポーネントは、写真アップロードのCSSクラス名で識別されます。 JavaScript関数は、アダプティブフォームに関連付けられたクライアントライブラリの一部です。 この関数は、添付ファイルコンポーネントのinitializeイベントで呼び出されます。

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### インライン画像の表示

ユーザーが画像をアップロードすると、添付ファイルコンポーネントのコミットイベントで次に示す関数が呼び出されます。 この関数は、アップロードされたファイルオブジェクトを引数として受け取ります。

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

* AEM package managerを使用して、AEMインスタンスに[クライアントライブラリ](assets/inline-image-client-library.zip)をダウンロードしてインストールします。
* AEM package managerを使用して、AEMインスタンスに[サンプルフォーム](assets/inline-image-af.zip)をダウンロードしてインストールします。
* ブラウザーに[追加インライン画像](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)を指定します
* 「写真を添付」ボタンをクリックして、画像を追加します