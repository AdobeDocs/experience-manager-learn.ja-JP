---
title: setData メソッドを使用してアダプティブフォームに入力
description: データ抽出用にアップロードされた PDF ファイルを送信し、抽出したデータをアダプティブフォームに入力します
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 14196
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: ht
source-wordcount: '126'
ht-degree: 100%

---

# Ajax 呼び出しを行う

ユーザーが PDF ファイルをアップロードした際、サーブレットに対して POST 呼び出しを行い、アップロードした PDF ドキュメントを POST リクエストで渡す必要があります。POST リクエストは、crx リポジトリ内の書き出されたデータへのパスを返す

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

**_/bin/ExtractDataFromPDF_** にマウントされたサーブレットは、PDF ファイルからデータを抽出し、抽出したデータが保存されている crx ノードのパスを返します。
次に、[GuideBridge setData](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javascript-api/GuideBridge.html#setData__anchor) メソッドを使用してアダプティブフォームのデータを設定します。

## 次の手順

[サンプルアセットのデプロイ](./test-the-solution.md)

