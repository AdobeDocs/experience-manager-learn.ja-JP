---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: アダプティブフォーム
type: Tutorial
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 2%

---

# クライアントライブラリの作成

AEMクライアントライブラリは、すべてのクライアント側JavaScriptコードを管理します。 この記事では、Guide Bridge APIを使用してアダプティブフォームのデータを取得する簡単なJavaScriptを作成しました。 アダプティブフォームのデータが取得されると、POSTの呼び出しがサーブレットに対しておこなわれ、アダプティブフォームのデータがデータベースに挿入または更新されます。 getALLUrlParams関数は、URL内のパラメーターを返します。 URLにguidパラメーターが存在する場合、更新操作を実行する必要があります（挿入操作でない場合）。残りの機能は、.savebuttonクラスのclickイベントに関連付けられたコードで処理されます。

>[!NOTE]
>
>クライアントライブラリは、このチュートリアルアセットの一部として提供されます

```javascript
function getAllUrlParams(url) {

    // get query string from url (optional) or window
    var queryString = url ? url.split('?')[1] : window.location.search.slice(1);

    // we'll store the parameters here
    var obj = {};

    // if query string exists
    if (queryString) {

        // stuff after # is not part of query string, so get rid of it
        queryString = queryString.split('#')[0];

        // split our query string into its component parts
        var arr = queryString.split('&');

        for (var i = 0; i < arr.length; i++) {
            // separate the keys and the values
            var a = arr[i].split('=');

            // set parameter name and value (use 'true' if empty)
            var paramName = a[0];
            var paramValue = typeof(a[1]) === 'undefined' ? true : a[1];

            // (optional) keep case consistent
            paramName = paramName.toLowerCase();
            if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();

            // if the paramName ends with square brackets, e.g. colors[] or colors[2]
            if (paramName.match(/\[(\d+)?\]$/)) {

                // create key if it doesn't exist
                var key = paramName.replace(/\[(\d+)?\]/, '');
                if (!obj[key]) obj[key] = [];

                // if it's an indexed array e.g. colors[2]
                if (paramName.match(/\[\d+\]$/)) {
                    // get the index value and add the entry at the appropriate position
                    var index = /\[(\d+)\]/.exec(paramName)[1];
                    obj[key][index] = paramValue;
                } else {
                    // otherwise add the value to the end of the array
                    obj[key].push(paramValue);
                }
            } else {
                // we're dealing with a string
                if (!obj[paramName]) {
                    // if it doesn't exist, create property
                    obj[paramName] = paramValue;
                } else if (obj[paramName] && typeof obj[paramName] === 'string') {
                    // if property does exist and it's a string, convert it to an array
                    obj[paramName] = [obj[paramName]];
                    obj[paramName].push(paramValue);
                } else {
                    // otherwise add the property
                    obj[paramName].push(paramValue);
                }
            }
        }
    }

    return obj;
}

$(document).ready(function() {
    console.log(window.location.hostname + "   " + window.location.protocol);
    var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
    var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
    linktext.visible = false;
    linktext1.visible = false;
    $(".savebutton").click(function() {
        var params = getAllUrlParams(window.location.href);
        console.log(getAllUrlParams(window.location.href));
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                console.log("The data is " + guideResultObject.data);
                let xhr = new XMLHttpRequest();
                xhr.open('POST', '/bin/storeafdata');
                let formData = new FormData();
                if (typeof(params.guid) != "undefined") {
                    formData.append("operation", "update");
                    formData.append("guid", params.guid);

                }
                if (typeof(params.guid) == "undefined") {
                    formData.append("operation", "insert");


                }


                formData.append("formdata", guideResultObject.data);
                xhr.send(formData);
                xhr.onload = function(e) {
                    console.log("The data is ready");
                    if (this.status == 200) {
                        var jsonResponse = JSON.parse(this.response);
                        console.log(jsonResponse.guid);
                        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                        linktext1.visible = true;
                        linktext.value =  "/content/dam/formsanddocuments/demostoreandretrieveformdata/jcr:content?wcmmode=disabled&guid=" + jsonResponse.guid;
                        linktext.visible = true;
                        guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                    }

                }
            }
        });


    });


});
```
