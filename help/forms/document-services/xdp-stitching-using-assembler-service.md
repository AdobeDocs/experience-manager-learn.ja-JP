---
title: Assembler サービスを使用した XDP ステッチ
description: AEM Forms の Assembler サービスを使用した XDP のステッチ
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 91
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '346'
ht-degree: 100%

---

# Assembler サービスを使用した XDP ステッチ

この記事では、Assembler サービスを使用して XDP ドキュメントをステッチする機能を示すアセットを提供します。
address.xdp という XDP ドキュメントから **address** というサブフォームを master.xdp ドキュメントの **address** という挿入ポイントに挿入するために、以下の JSP コードを作成しました。結果の XDP は、AEM インストール環境のルートフォルダーに保存されました。

Assembler サービスは、PDF ドキュメントの操作を記述する有効な DDX ドキュメントに準拠しています。こちらの [DDX のリファレンスドキュメント](assets/ddxRef.pdf)を参照してください。40 ページに XDP ステッチに関する情報があります。

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

フラグメントを別の XDP に挿入する DDX ファイルを以下に示します。DDX は address.xdp のサブフォーム **address** を master.xdp の **address** という挿入ポイントに挿入します。**stitched.xdp** という名前のドキュメントが結果として生成され、ファイルシステムに保存されます。

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

この機能を AEM Server で動作させるには

* [XDP Stitching パッケージ](assets/xdp-stitching.zip)をローカルシステムにダウンロードします。
* [パッケージマネージャ](http://localhost:4502/crx/packmgr/index.jsp)を使用して、このパッケージをアップロードしインストールします。
* [この zip ファイルの内容を展開](assets/xdp-and-ddx.zip)すると、サンプル XDP ファイルと DDX ファイルが得られます。

**パッケージのインストール後、Adobe Granite CSRF Filter で次の URL を許可リストに加える必要があります**。

1. 上記のパスを許可リストに加えるには、下記の手順に従ってください。
1. [configMgr にログインします](http://localhost:4502/system/console/configMgr)。
1. Adobe Granite CSRF フィルターを検索します。
1. 「除外済み」セクションに次のパスを追加し、`/content/AemFormsSamples/assemblerservice` を保存します。
1. 「Sling Referrer Filter」を検索します。
1. 「Allow Empty」チェックボックスをオンにします（この設定はテスト目的でのみ使用してください）。
サンプルコードをテストする方法は多数あります。 Postman アプリを使用するのが最もすばやく簡単です。Postman を使用すると、サーバーに POST リクエストを送信できます。 システムに Postman アプリをインストールします。
アプリを起動し次の URL を入力して、データ書き出し API をテストします。
http://localhost:4502/content/AemFormsSamples/assemblerservice.html

スクリーンショットに示したとおりに、入力パラメーターを指定します。前にダウンロードしたサンプルドキュメントを使用できます。
![xdp-stitch-postman](assets/xdp-stitching-postman.png)

>[!NOTE]
>
>AEM Forms のインストールが完了していることを確認します。すべてのバンドルがアクティブ状態である必要があります。
>
