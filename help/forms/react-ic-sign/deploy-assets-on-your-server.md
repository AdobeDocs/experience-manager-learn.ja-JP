---
title: サーバーへのサンプルアセットのデプロイ
description: ローカルサーバーで動作するユースケースの取得
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '148'
ht-degree: 100%

---

# アセットのデプロイ

次のアセットまたは設定が、AEM Forms パブリッシュサーバーにデプロイされました。

* [Adobe Sign ラッパーバンドル](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [インタラクティブ通信のサンプルテンプレート](assets/waiver-interactive-communication.zip)
* [DevelopingWithServiceUser バンドルのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* OSGi configMgr を使用して、Apache Sling Service User Mapper Service に次のエントリを追加します。
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [サンプルの React アプリコードは、こちらからダウンロードできます](assets/src.zip)



サンプルの React アプリをローカル環境にデプロイする必要があります。

環境に合わせてエンドポイント URL を変更する必要があります。EmergencyContact.js ファイルを開き、fetch メソッド内の URL を変更します。

```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

React アプリから AEM エンドポイントへの POST 呼び出しを有効にするには、Adobe Granite クロスオリジンリソース共有ポリシー設定の「許可されたオリジン」フィールドで適切なエントリを指定する必要があります。

![cors-setting](assets/cors-settings.png)
