---
title: Web ページへのアダプティブフォームまたは HTML5 フォームの埋め込み
description: アダプティブフォームまたは HTML5 フォームを AEM 以外の web ページに埋め込むために必要な設定手順です。
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 53af8fbc20ff21abf8778bbc165b5ec7fbdf8c8f
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 80%

---

# Web ページへのアダプティブフォームまたは HTML5 フォームの埋め込み

埋め込まれたアダプティブフォームではすべての機能を使用できるので、ユーザーは、ページから移動することなくフォームを記入および送信できます。これにより、ユーザーは Web ページ上の他の要素のコンテキストにとどまり、同時にフォームを操作できます。

次のビデオでは、web ページにアダプティブフォームまたはHTML5 フォームを埋め込むために必要な手順を説明しています。
最適な前提条件やベストプラクティスなどについては、[ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html)を参照してください。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

ビデオで使用されているサンプルファイルは、[こちらから](assets/embedding-af-web-page.zip)ダウンロードできます。

アダプティブフォームを取得し、クラス名 **right** で識別されるコンテナにフォームを埋め込むためのコードを以下に示します。

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
