---
title: WebページへのアダプティブForms/HTML5フォームの埋め込み
description: アダプティブFormsまたはHTML5フォームをAEM以外のWebページに埋め込むために必要な設定手順。
feature: アダプティブフォーム
feature-set: Adaptive Forms
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: 管理
role: Admin
level: Beginner
kt: 8390
source-git-commit: 2fc4f748fd3b8f820d1451d08c5fe01d11892029
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 28%

---


# WebページへのアダプティブフォームまたはHTML5フォームの埋め込み

埋め込まれたアダプティブフォームではすべての機能を使用できるため、ユーザーは、ページから移動することなくフォームを記入および送信できます。これにより、ユーザーは Web ページのその他のエレメントから離れることなく、同時にフォームの操作を行うことができます。

次のビデオでは、アダプティブフォームまたはHTML5フォームをWebページに埋め込むために必要な手順を説明します。
ベストプラクティス、ベストプラクティスなどについては、[ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-64/forms/adaptive-forms-basic-authoring/embed-adaptive-form-external-web-page.html?lang=en)を参照してください。
>[!VIDEO](https://video.tv.adobe.com/v/335893?quality=9&learn=on)

ビデオ[で使用されているサンプルファイルは、こちらからダウンロードできます。](assets/embedding-af-web-page.zip)

次に、アダプティブフォームを取得し、クラス名&#x200B;**right**&#x200B;で識別されるコンテナにフォームを埋め込むためのコードを示します

```javascript
$(document).ready(function(){
  
	var formName = $( "#xfaform" ).val();
    $.get("http://localhost/content/dam/formsanddocuments/newslettersubscription/jcr:content?wcmmode=disabled", function(data, status){
	console.log(formName);
	$(".right").append(data);
      
    });
  
});
```














