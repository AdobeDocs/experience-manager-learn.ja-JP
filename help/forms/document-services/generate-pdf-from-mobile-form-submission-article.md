---
title: HTM5 フォーム送信から PDF を生成
description: モバイルフォームの送信から PDF を生成
feature: Mobile Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 91b4a134-44a7-474e-b769-fe45562105b2
last-substantial-update: 2020-01-07T00:00:00Z
duration: 132
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '517'
ht-degree: 100%

---

# HTM5 フォーム送信から PDF を生成 {#generate-pdf-from-htm-form-submission}

この記事では、HTML5（Mobile Forms）フォームの送信から PDF を生成する手順について説明します。このデモでは、画像を HTML5 フォームに追加し、画像を最終的な PDF に結合するために必要な手順も説明します。


送信されたデータを XDP テンプレートに結合するには、次の手順を実行します。

HTML5 のフォーム送信を処理するサーブレットを作成する

* このサーブレット内で、送信されたデータを取得する
* このデータを XDP テンプレートと結合して PDF を生成する
* PDF を呼び出し元のアプリケーションにストリーミングする

次に、リクエストから送信されたデータを抽出するサーブレットコードを示します。次に、カスタム documentServices .mobileFormToPDF メソッドを呼び出して PDF を取得します。

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

モバイルフォームに画像を追加し、その画像を PDF に表示するには、次を使用します。

XDP テンプレート - XDP テンプレートに btnAddImage という画像フィールドとボタンを追加しました。次のコードは、カスタムプロファイル内の btnAddImage のクリックイベントを処理します。このスライドに示すように、「ファイル 1 クリック」イベントのトリガーは次のとおりです。このユースケースを実現するために XDP でコーディングは必要ありません。

```javascript
$(".btnAddImage").click(function(){

$("#file1").click();

});
```

[カスタムプロファイル](https://helpx.adobe.com/jp/livecycle/help/mobile-forms/creating-profile.html#CreatingCustomProfiles)。カスタムプロファイルを使用すると、モバイルフォームの HTML DOM オブジェクトの操作が容易になります。非表示のファイル要素が HTML.jsp に追加されます。ユーザーが「写真を追加」をクリックすると、ファイル要素のクリックイベントがトリガーされます。これにより、ユーザーは添付する写真を参照して選択できます。次に、JavaScript の FileReader オブジェクトを使用して、画像の base64 エンコードされた文字列を取得します。base64 画像文字列は、フォームのテキストフィールドに格納されます。フォームが送信されると、この値を抽出し、XML の img 要素に挿入します。その後、この XML を使用して XDP とマージし、最終的な PDF を生成します。

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

上記のコードは、file 要素のクリックイベントをトリガーすると実行されます。5 行目では、アップロードされたファイルの内容を base64 文字列として抽出し、テキストフィールドに格納します。この値は、フォームがサーブレットに送信される際に抽出されます。

次に、AEM でモバイルフォームの次のプロパティ（詳細）を設定します。

* URL を送信 - http://localhost:4502/bin/handlemobileformsubmissionこれは、送信されたデータを XDP テンプレートと結合するサーブレットです。
* HTML レンダリングプロファイル - 「AddImageToMobileForm」を選択していることを確認します。これにより、トリガーを使用して画像をフォームに追加します。

ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

* [AemFormsDocumentServices バンドルをデプロイ](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)

* [サービスユーザーバンドルを使用した開発のデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [この記事に関連するパッケージをダウンロードしてインストールします。](assets/pdf-from-mobile-form-submission.zip)

* [XDP](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/schengen.xdp) のプロパティページを表示して、送信 URL と HTML レンダリングプロファイルが正しく設定されていることを確認します。

* [XDP を HTML としてプレビュー](http://localhost:4502/content/dam/formsanddocuments/schengen.xdp/jcr:content)

* 画像をフォームに追加して送信します。画像を含む PDF が返されます。
