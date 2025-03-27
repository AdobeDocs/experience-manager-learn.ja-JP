---
title: setData メソッドを使用してアダプティブフォームに入力
description: データ抽出用にアップロードされた PDF ファイルを送信し、抽出したデータをアダプティブフォームに入力します
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-14196
exl-id: f380d589-6520-4955-a6ac-2d0fcd5aaf3f
duration: 32
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '118'
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
