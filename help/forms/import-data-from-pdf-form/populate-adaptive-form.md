---
title: setData メソッドを使用してアダプティブフォームに入力する
description: データ抽出用にアップロードされた PDF ファイルを送信し、抽出したデータをアダプティブフォームに入力します
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 4%

---

# Ajax 呼び出しを行う

ユーザーが pdf ファイルをアップロードしたら、POST呼び出しをサーブレットに対しておこない、アップロードしたPDFドキュメントをPOSTリクエストに渡す必要があります。 POSTリクエストは、crx リポジトリ内の書き出されたデータへのパスを返します。

```javascript
$("#fileElem").on('change', function (e) {
           console.log("submitting files");
           var filesUploaded = e.target.files;
           var ajaxData = new FormData($("#myform").get(0));
           for (var i = 0; i < filesUploaded.length; i++) {
               ajaxData.append(filesUploaded[i].name, filesUploaded[i]);
           }

           handleFiles(ajaxData);

       });

function handleFiles(formData) {
    console.log("File uploaded");

    $.ajax({
        type: 'POST',
        data: formData,
        url: '/bin/ExtractDataFromPDF',
        contentType: false,
        processData: false,
        cache: false,
        success: function (filePath) {
            console.log(filePath);
            guideBridge.setData({
                dataRef: filePath,
                error: function (guideResultObject) {
                    console.log("Error");
                }
            })
            

        }
    });
}
```

次にマウントされたサーブレット： **_/bin/ExtractDataFromPDF_** は、PDFファイルからデータを抽出し、抽出したデータが保存されている crx ノードのパスを返します。
The [GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) メソッドを使用して、アダプティブフォームのデータを設定します。

## 次の手順

[サンプルアセットのデプロイ](./test-the-solution.md)


