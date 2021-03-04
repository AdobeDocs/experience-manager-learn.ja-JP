---
title: HTM5フォーム送信からPDFを生成
seo-title: HTML5フォーム送信からPDFを生成
description: Mobile FormからPDFを生成送信
seo-description: Mobile FormからPDFを生成送信
uuid: 61f07029-d440-44ec-98bc-f2b5eef92b59
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
discoiquuid: 816f1a75-6ceb-457b-ba18-daf229eed057
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '583'
ht-degree: 0%

---


# HTM5フォーム送信からPDFを生成{#generate-pdf-from-htm-form-submission}

この記事では、HTML5(モバイルForms)フォームの送信からpdfを生成する手順について説明します。 また、このデモでは、HTML5フォームに画像を追加し、画像を最終的なpdfに結合するために必要な手順について説明します。

この機能の実況デモを見るには、[サンプルサーバー](https://forms.enablementadobe.com/content/samples/samples.html?query=0)にアクセスし、「Mobile Form To PDF」を検索してください。

送信されたデータをxdpテンプレートに結合するには、次の手順を実行します

HTML5フォーム送信を処理するサーブレットの作成

* このサーブレット内で、送信されたデータを取得します
* このデータをxdpテンプレートと結合してpdfを生成します
* PDFを呼び出し元のアプリケーションにストリーミング戻す

以下は、リクエストから送信データを抽出するサーブレットコードです。 次に、カスタムdocumentServices .mobileFormToPDFメソッドを呼び出してPDFを取得します。

```java
protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
  StringBuffer stringBuffer = new StringBuffer();
  String line = null;
  try {
   InputStreamReader isReader = new InputStreamReader(request.getInputStream(), "UTF-8");
   BufferedReader reader = new BufferedReader(isReader);
   while ((line = reader.readLine()) != null)
    stringBuffer.append(line);
  } catch (Exception e) {
   System.out.println("Error");
  }
  String xmlData = new String(stringBuffer);
  Document generatedPDF = documentServices.mobileFormToPDF(xmlData);
  try {
   
   InputStream fileInputStream = generatedPDF.getInputStream();
   response.setContentType("application/pdf");
   response.addHeader("Content-Disposition", "attachment; filename=AemFormsRocks.pdf");
   response.setContentLength((int) fileInputStream.available());
   OutputStream responseOutputStream = response.getOutputStream();
   int bytes;
   while ((bytes = fileInputStream.read()) != -1) {
    responseOutputStream.write(bytes);
   }
   responseOutputStream.flush();
   responseOutputStream.close();

  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }
```

モバイルフォームに画像を追加し、その画像をPDFに表示するには、次の手順を実行します

XDPテンプレート — xdpテンプレートに、btnAddImageという画像フィールドとボタンを追加しました。 次のコードは、カスタムプロファイルでのbtnAddImageのクリックイベントを処理します。 表示されているように、file1 clickイベントをトリガーします。 この使用例を達成するためにxdpでコーディングする必要はありません

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[カスタムプロファイル](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)。カスタムプロファイルを使用すると、モバイルフォームのHTML DOMオブジェクトを簡単に操作できます。 非表示のファイル要素がHTML.jspに追加されます。 ユーザーが「追加Your Photo」をクリックすると、ファイル要素のclickイベントがトリガーされます。 これにより、添付する写真をユーザが参照して選択できます。 次に、javascript FileReaderオブジェクトを使用して、画像のbase64エンコードされた文字列を取得します。 base64画像文字列は、フォームのテキストフィールドに格納されます。 フォームが送信されたら、この値を抽出し、XMLのimg要素に挿入します。 次に、このXMLを使用してxdpとマージし、最終的なpdfを生成します。

この記事に使用するカスタムプロファイルは、この記事のアセットの一部として利用できるようになっています。

```javascript
function readURL(input) {
            if (input.files && input.files[0]) {
                var reader = new FileReader();
                reader.onload = function (e) {
                  window.formBridge.setFieldValue("xfa.form.topmostSubform.Page1.base64image",reader.result);
                    $('.img img').show();
                     $('.img img')
                        .attr('src', e.target.result)
                        .width(180)
                        .height(200)
                };

                reader.readAsDataURL(input.files[0]);
            }
        }
```

上記のコードは、file要素のclickイベントをトリガーすると実行されます。 5行目では、アップロードされたファイルの内容をbase64文字列として抽出し、テキストフィールドに格納します。 この値は、フォームがサーブレットに送信されたときに抽出されます。

その後、AEMでモバイルフォームの次のプロパティ（詳細）を設定します

* 送信URL - http://localhost:4502/bin/handlemobileformsubmission。 これは、送信されたデータをxdpテンプレートと統合するサーブレットです
* HTMLレンダリングプロファイル- 「AddImageToMobileForm」を選択していることを確認します。 これにより、フォームに画像を追加するコードがトリガーされます。

ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

* [AemFormsDocumentServicesバンドルのデプロイ](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [サービスユーザーバンドルを使用した開発の展開](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [この記事に関連付けられたパッケージをダウンロードしてインストールします。](assets/pdf-from-mobile-form-submission.zip)

* [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)のプロパティページを表示して、送信URLとHTMLレンダリングプロファイルが正しく設定されていることを確認します

* [XDPをhtmlとしてプレビューする](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 画像追加をフォームに送信します。 画像を含むPDFが返されます。

