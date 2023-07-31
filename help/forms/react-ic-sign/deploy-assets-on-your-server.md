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
workflow-type: ht
source-wordcount: '149'
ht-degree: 100%

---

# アセットのデプロイ

次のアセットまたは設定が、AEM Forms パブリッシュサーバーにデプロイされました。

* [Adobe Sign ラッパーバンドル](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [インタラクティブ通信のサンプルテンプレート](assets/waiver-interactive-communication.zip)
* [DevelopingWithServiceUser バンドルのデプロイ](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* OSGi configMgr を使用して、Apache Sling Service User Mapper Service に次のエントリを追加します。
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## サンプル React アプリをデプロイ

* [サンプル React アプリをダウンロードします](assets/mult-step-form1.zip)
* React アプリのコンテンツを新しいフォルダーに解凍します
* フォルダーに移動し、次のコマンドを実行します

```java
npm install
npm start
```

EmergencyContact.js ファイルを開き、環境に合わせて fetch メソッド内の URL を変更します。


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
