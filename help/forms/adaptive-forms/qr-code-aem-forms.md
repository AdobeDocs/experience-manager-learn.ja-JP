---
title: アダプティブフォームでの QR コードの表示
description: アダプティブフォームでの QR コードの表示
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
exl-id: 0c6079f4-601e-4a82-976c-71dbb2faa671
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '203'
ht-degree: 100%

---

# サンプルの QR コードコンポーネント

アダプティブフォームに QR コードを埋め込むと、ユーザーがフォームに関連する追加情報にアクセスする際の利便性と効率を大幅に向上させることができます。

サンプルコンポーネントは [QRCode.js](https://davidshimjs.github.io/qrcodejs/) を利用します。

QRCode.js は QRCode を作成するための JavaScript ライブラリで、HTML5 Canvas と DOM の table タグによるクロスブラウザーをサポートします。

このコンポーネントは、コンポーネントの設定プロパティで指定された値に基づいて QR コードを生成します。
![画像](assets/qr-code-url.png)

次のコードは、qr-code-generator コンポーネントの body.jsp で使用されていました。

「URL」は、QR コードに埋め込む必要がある URL です。この URL は、QR コードコンポーネントの設定プロパティで指定されます。

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



次のコードは、qr-code-generator コンポーネントのクライアントライブラリで、QRCode.js ライブラリの makeCode メソッドを利用します。生成された QR コードは、id **&quot;qrcode&quot;** で識別される div に追加されます。

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## ローカルサーバーへのアセットのデプロイ

* [パッケージマネージャーを使用して QR コードコンポーネントをダウンロードしてインストールします。](assets/qrcode.zip)
* [パッケージマネージャーを使用して、サンプルアダプティブフォームをダウンロードおよびインストールします。](assets/form-with-qr-code.zip)
* [フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled)。フォームのヘルプセクションに QR コードがあります。
