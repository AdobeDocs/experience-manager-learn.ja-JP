---
title: HTM5 フォーム送信からPDFを生成
description: モバイルフォームの送信からPDFを生成
feature: Mobile Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '544'
ht-degree: 0%

---

# HTM5 フォーム送信からPDFを生成 {#generate-pdf-from-htm-form-submission}

この記事では、HTML5(Mobile Forms) フォームの送信から pdf を生成する手順について説明します。 このデモでは、画像をHTML5 フォームに追加し、画像を最終的な pdf に結合するために必要な手順も説明します。


送信されたデータを xdp テンプレートに結合するには、次の手順を実行します

Servlet5 フォーム送信を処理するHTMLを書き込む

* このサーブレット内で、送信されたデータを取得します
* このデータを xdp テンプレートと結合して pdf を生成します
* PDF を呼び出し元のアプリケーションにストリーミングします。

次に、リクエストから送信されたデータを抽出するサーブレットコードを示します。 次に、カスタム documentServices .mobileFormToPDF メソッドを呼び出して PDF を取得します。

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

モバイルフォームに画像を追加し、その画像を PDF に表示するには、以下を使用します

XDP テンプレート — xdp テンプレートに btnAddImage という画像フィールドとボタンを追加しました。 次のコードは、カスタムプロファイル内の btnAddImage の click イベントを処理します。 このスライドに示すように、file1 click イベントのトリガーは次のとおりです。 この使用例を実現するために xdp でコーディングは必要ありません

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[カスタムプロファイル](https://helpx.adobe.com/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles). カスタムプロファイルを使用すると、モバイルフォームのHTMLDOM オブジェクトの操作が容易になります。 非表示のファイル要素がHTML.jsp に追加されます。 ユーザーが「写真を追加」をクリックすると、ファイル要素の click イベントがトリガーされます。 これにより、ユーザは添付する写真を参照して選択できます。 次に、JavaScript の FileReader オブジェクトを使用して、画像の base64 エンコードされた文字列を取得します。 base64 画像文字列は、フォームのテキストフィールドに格納されます。 フォームが送信されると、この値を抽出し、XML の img 要素に挿入します。 その後、この XML を使用して xdp とマージし、最終的な PDF を生成します。

この記事で使用するカスタムプロファイルは、この記事のアセットの一部として使用できるようになっています。

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

上記のコードは、ファイル要素の click イベントをトリガーすると実行されます。 5 行目では、アップロードされたファイルの内容を base64 文字列として抽出し、テキストフィールドに格納します。 この値は、フォームがサーブレットに送信される際に抽出されます。

次に、AEMでモバイルフォームの次のプロパティ（詳細）を設定します

* URL を送信 — http://localhost:4502/bin/handlemobileformsubmission. これは、送信されたデータを xdp テンプレートと結合するサーブレットです
* HTMLレンダリングプロファイル — 「AddImageToMobileForm」を選択していることを確認します。 これにより、トリガーを使用して画像をフォームに追加します。

ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

* [AemFormsDocumentServices バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [Developing with Service User バンドルのデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [この記事に関連するパッケージをダウンロードしてインストールします。](assets/pdf-from-mobile-form-submission.zip)

* 送信 URL とHTMLレンダリングプロファイルが正しく設定されていることを確認するには、  [xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp)

* [XDP を html としてプレビュー](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 画像をフォームに追加して送信します。 画像を含むPDFが返されます。
