---
title: Web ページへのアダプティブForms/HTML5 フォームの埋め込み
description: アダプティブFormsまたはHTML5 フォームをAEM以外の Web ページに埋め込むために必要な設定手順。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 8390
exl-id: 068e38df-9c71-4f55-b6d6-e1486c29d0a9
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 13%

---

# Web ページへのアダプティブフォームまたはHTML5 フォームの埋め込み

埋め込まれたアダプティブフォームではすべての機能を使用できるため、ユーザーは、ページから移動することなくフォームを記入および送信できます。これにより、ユーザーは Web ページ上の他の要素のコンテキストにとどまり、同時にフォームを操作できます。

次のビデオでは、Web ページにアダプティブフォームまたはHTML5 フォームを埋め込むために必要な手順を説明します。
詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en) ベストプラクティス、ベストプラクティスなど
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=12&learn=on)

このビデオで使用するサンプルファイルをダウンロードできます [ここから](assets/embedding-af-web-page.zip)

次に、アダプティブフォームを取得し、クラス名で識別されるコンテナにフォームを埋め込むためのコードを示します **右**

```javascript
$(document).ready(function(){
  
    var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
    console.log(formName);
    $(".right").append(data);
      
    });
  
});
```
