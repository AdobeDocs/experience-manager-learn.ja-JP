---
title: Web ページへのアダプティブフォームまたは HTML5 フォームの埋め込み
description: アダプティブフォームまたは HTML5 フォームを AEM 以外の web ページに埋め込むために必要な設定手順です。
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
duration: 398
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 100%

---

# Web ページへのアダプティブフォームまたは HTML5 フォームの埋め込み

埋め込まれたアダプティブフォームではすべての機能を使用できるので、ユーザーは、ページから移動することなくフォームを記入および送信できます。これにより、ユーザーは web ページの他の要素から離れることなく、同時にフォームの操作も行うことができます。

次のビデオでは、web ページにアダプティブフォームまたはHTML5 フォームを埋め込むために必要な手順を説明しています。
最適な前提条件やベストプラクティスなどについては、[ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=ja)を参照してください。
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
