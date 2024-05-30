---
title: アダプティブフォームでの QR コードの表示
description: アダプティブフォームでの QR コードの表示
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
source-git-commit: e20d9f80cc7e1c6f5f6c81233d9a5178551e2fa2
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 1%

---

# サンプルの QR コードコンポーネント

アダプティブフォームに QR コードを埋め込むと、ユーザーがフォームに関連する追加情報にアクセスする際の利便性と効率が大幅に向上します。

サンプルコンポーネントは以下を利用します [QRCode.js](https://davidshimjs.github.io/qrcodejs/).

QRCode.js は、QRCode を作成するための JavaScript ライブラリで、DOM 内で Canvas と table タグをHTML5 で使用するクロスブラウザーをサポートします。

このコンポーネントは、コンポーネントの設定プロパティで指定された値に基づいて QR コードを生成します。
![画像](assets/qr-code-url.png)

次のコードは、qr-code-generator コンポーネントの body.jsp で使用されています。

「url」は、qr コードに埋め込む必要がある url です。 この URL は、QR コードコンポーネントの設定プロパティで指定されます。

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



次のコードは、qr-code-generator コンポーネントのクライアントライブラリで、QRCode.js ライブラリの makeCode メソッドを利用しています。生成された QR コードは、id で識別される div に追加されます **&quot;qrcode&quot;**.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## ローカルサーバーへのアセットのデプロイ

* [パッケージマネージャーを使用して QR コードコンポーネントをダウンロードしてインストールします。](assets/qrcode.zip)
* [パッケージマネージャーを使用して、サンプルのアダプティブフォームをダウンロードしてインストールします。](assets/form-with-qr-code.zip)
* [フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled)。フォームのヘルプセクションには QR コードがあります。


