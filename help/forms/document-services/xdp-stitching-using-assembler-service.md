---
title: Assembler サービスを使用した XDP ステッチ
description: AEM Formsで Assembler サービスを使用した xdp のステッチ
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
source-git-commit: 8f17e98c56c78824e8850402e3b79b3d47901c0b
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 2%

---

# Assembler サービスを使用した XDP のステッチ

この記事では、Assembler サービスを使用して xdp ドキュメントをステッチする機能を示すアセットを提供します。
次の JSP コードは、という名前のサブフォームを挿入するために記述されました。 **住所** address.xdp という xdp ドキュメントから **住所** master.xdp ドキュメント内 結果の xdp は、AEMインストール環境のルートフォルダーに保存されました。

Assembler サービスは、有効な DDX ドキュメントに基づいて、PDFドキュメントの操作を記述します。 以下を参照してください。 [DDX リファレンスドキュメントはこちら](assets/ddxRef.pdf).Page 40 は xdp ステッチに関する情報を持ちます。

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

フラグメントを別の xdp に挿入する DDX ファイルを以下に示します。 DDX がサブフォームを挿入します  **住所** address.xdp から **住所** master.xdp 内 次の名前の結果ドキュメント： **stitched.xdp** はファイルシステムに保存されます。

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

この機能をAEM Server で動作させるには

* ダウンロード [XDP Stitching パッケージ](assets/xdp-stitching.zip) をローカルシステムに送信します。
* を使用してパッケージをアップロードしインストールする [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* [この zip ファイルの内容を抽出します。](assets/xdp-and-ddx.zip) サンプル xdp および DDX ファイルを取得するには

**パッケージをインストールした後、Granite CSRF FilterAdobeに次の URL をする必要がありま許可リストす。**

1. 上記のパスをするには、以下の手許可リスト順に従ってください。
1. [configMgr にログイン](http://localhost:4502/system/console/configMgr)
1. AdobeGranite CSRF Filter を検索します。
1. 除外されたセクションに次のパスを追加して、保存します。 `/content/AemFormsSamples/assemblerservice`
1. 「Sling Referrer filter」を検索します。
1. 「Allow Empty」チェックボックスをオンにします。 （この設定はテスト目的でのみ使用する必要があります）サンプルコードをテストする方法は多数あります。 Postmanアプリを最もすばやく最も簡単に使用できます。 Postmanを使用すると、サーバーにPOSTリクエストを送信できます。 システムにPostmanアプリをインストールします。
デスクトップアプリケーションを起動し、次の URL を入力して、書き出しデータ API をテストしますhttp://localhost:4502/content/AemFormsSamples/assemblerservice.html

スクリーンショットで指定した次の入力パラメーターを指定します。 前にダウンロードしたサンプルドキュメントを使用できます。
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>AEM Formsのインストールが完了していることを確認します。 すべてのバンドルがアクティブ状態である必要があります。
