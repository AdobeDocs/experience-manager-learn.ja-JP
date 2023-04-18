---
title: サーバーにサンプルアセットをデプロイします。
description: ローカルサーバーでのユースケースの動作
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
source-git-commit: 155e6e42d4251b731d00e2b456004016152f81fe
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# アセットのデプロイ

次のアセット/設定が、AEM Formsパブリッシュサーバーにデプロイされました。

* [Adobe Sign Wrapper Bundle](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [インタラクティブ通信テンプレートの例](assets/waiver-interactive-communication.zip)
* [DevelopingWithServiceUser バンドルをデプロイします。](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* OSGi configMgr を使用して、Apache Sling Service User Mapper Service に次のエントリを追加します。
   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**
* [サンプルの React アプリコードは、こちらからダウンロードできます。](assets/src.zip)



サンプルの React アプリをローカル環境にデプロイする必要があります

環境に合わせてエンドポイント URL を変更する必要があります。 EmergencyContact.js ファイルを開き、fetch メソッドで URL を変更します。

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

React アプリからAEMエンドポイントに対してPOST呼び出しを有効にするには、AdobeGranite クロスオリジンリソース共有ポリシー設定の「許可されたオリジン」フィールドに適切なエンティティを指定する必要があります

![cors-setting](assets/cors-settings.png)



