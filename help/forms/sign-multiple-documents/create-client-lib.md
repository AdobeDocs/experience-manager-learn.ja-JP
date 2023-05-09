---
title: クライアントライブラリの作成
description: 署名する次のフォームを取得するクライアントライブラリコード
feature: Adaptive Forms
version: 6.4,6.5
kt: 6907
thumbnail: 6907.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 3c148b30-2c7d-428d-9a3c-f3067ca3a239
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 91%

---

# クライアントライブラリの作成

URL パラメーターを抽出して GET 呼び出しで渡すカスタムクライアントライブラリ（clientlib）を作成します。この GET 呼び出しは、/bin/getnextformtosign にマウントされたサーブレットに対して行われ、このサーブレットは、パッケージで署名する次のフォームの URL を返します。

clientlib JavaScript 関数で使用されるコードは次のとおりです。


```java
function getUrlVars()
{
    var vars = {};
    var parts = window.location.href.replace(/[?&]+([^=&]+)=([^&]*)/gi, function(m, key, value)
    {
        vars[key] = value;
    });
    return vars;
}

function navigateToNextForm()
{
    
    console.log("The id is " + guidelib.runtime.adobeSign.submitData.agreementId);
    var guid = getUrlVars()["guid"];
    var customerID = getUrlVars()["customerID"];
    console.log("The customer Id is " + customerID);
    $.ajax(
    {
        type: 'GET',
        url: '/bin/getnextformtosign?guid=' + guid + '&customerID=' + customerID,
        contentType: false,
        processData: false,
        cache: false,
        success: function(response)
        {
            console.log(response);
            var jsonResponse = JSON.parse(JSON.stringify(response));
            console.log(jsonResponse.nextFormToSign);
            var nextFormToSign = jsonResponse.nextFormToSign;
            if (nextFormToSign != "AllDone")
            {
                window.open(nextFormToSign, '_self');
            }
            else
            {
                window.open("http://localhost:4502/content/forms/af/formsandsigndemo/alldone.html", '_self');
            }

}
    });
}
$(document).ready(function()
{
    $(document).on("click", ".nextform", navigateToNextForm);
});
```

## Assets

[clientlib は、こちらからダウンロードできます。](assets/get-next-form-client-lib.zip)

## 次の手順

[このユースケースで使用するカスタムフォームテンプレートの作成](./create-af-template.md)