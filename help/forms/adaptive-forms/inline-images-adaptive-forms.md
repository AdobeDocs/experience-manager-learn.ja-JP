---
title: アダプティブフォームでのインライン画像の表示
description: アップロードした画像をアダプティブフォームにインラインで表示する
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
last-substantial-update: 2020-06-09T00:00:00Z
duration: 58
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '216'
ht-degree: 100%

---

# アダプティブフォームのインライン画像

一般的な使用例として、アップロードした画像をアダプティブフォーム内のインライン画像として表示する場合があります。デフォルトでは、アップロードされた画像はリンクとして表示され、画像をアダプティブフォームに表示することで、このエクスペリエンスを強化できます。この記事では、インライン画像の表示に関する手順について説明します。

## プレースホルダー画像の追加

最初の手順では、添付ファイルコンポーネントにプレースホルダー div を追加します。次のコードでは、添付ファイルコンポーネントは、photo-upload の CSS クラス名で識別されます。JavaScript 関数は、アダプティブフォームに関連付けられているクライアントライブラリの一部です。この関数は、添付ファイルコンポーネントの initialize イベントで呼び出されます。

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

ユーザーが画像をアップロードすると、次に示す関数が、ファイル添付コンポーネントの commit イベントで呼び出されます。関数は、アップロードされたファイルオブジェクトを引数として受け取ります。

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

### サーバーへのデプロイ

* AEM パッケージマネージャーを使用して、AEM インスタンスに[クライアントライブラリ](assets/inline-image-client-library.zip)をダウンロードし、インストールします。
* AEM パッケージマネージャーを使用して、[サンプルフォーム](assets/inline-image-af.zip)をダウンロードし、AEM インスタンスにインストールします。
* ブラウザーで[インライン画像の追加](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)を指定します。
* 「写真を添付」ボタンをクリックして画像を追加する
