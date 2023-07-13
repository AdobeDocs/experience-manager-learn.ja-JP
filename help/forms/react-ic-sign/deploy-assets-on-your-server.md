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
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 46%

---

# アセットのデプロイ

次のアセットまたは設定が、AEM Forms パブリッシュサーバーにデプロイされました。

* [Adobe Sign ラッパーバンドル](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [インタラクティブ通信のサンプルテンプレート](assets/waiver-interactive-communication.zip)
* [DevelopingWithServiceUser バンドルのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* OSGi configMgr を使用して、Apache Sling Service User Mapper Service に次のエントリを追加します。
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## サンプル React アプリのデプロイ

* [サンプル React アプリのダウンロード](assets/mult-step-form1.zip)
* React アプリのコンテンツを新しいフォルダーに解凍します。
* フォルダーに移動し、次のコマンドを実行します。

```java
npm install
npm start
```

EmergencyContact.js ファイルを開き、fetch メソッドの URL を環境に合わせて変更します。


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

React アプリからAEMエンドポイントに対してPOST呼び出しを有効にするには、AdobeGranite クロスオリジンリソース共有ポリシー設定の「許可されたオリジン」フィールドに適切なエンティティを指定する必要があります。

![cors-setting](assets/cors-settings.png)
