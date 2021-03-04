---
title: 1つのデータファイルからの複数のPDFの生成
seo-title: 1つのデータファイルからの複数のPDFの生成
feature: Output サービス
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '501'
ht-degree: 2%

---


# 1つのxmlデータファイルからPDFドキュメントのセットを生成する

OutputServiceには、フォームデザインを使用してドキュメントを作成する方法と、フォームデザインとマージするデータが多数用意されています。 次の記事では、複数の個々のレコードを含む1つの大きなxmlから複数のpdfを生成する使用例を説明します。
複数のレコードを含むxmlファイルのスクリーンショットを次に示します。

![multi-record-xml](assets/multi-record-xml.PNG)

データxmlには2つのレコードがあります。 各レコードは、form1要素で表されます。 このxmlはOutputService [generatePDFOutputBatchメソッド](https://helpx.adobe.com/aem-forms/6/javadocs/com/adobe/fd/output/api/OutputService.html)に渡され、pdfドキュメントのリストが取得されます（レコードごとに1つ）
generatePDFOutputBatchメソッドの署名は、次のパラメーターを使用します

* templates — キーで識別されるテンプレートを含むマップ
* data — キーで識別されるxmlデータドキュメントを含むマップ
* pdfOutputOptions - pdf生成を設定するためのオプション
* batchOptions — バッチを設定するためのオプション

>[!NOTE]
>
>この使用例は、この[webサイト](https://forms.enablementadobe.com/content/samples/samples.html?query=0)で実例として参照できます。

## 使用事例の詳細{#use-case-details}

この使用事例では、テンプレートとデータ(xml)ファイルをアップロードするためのシンプルなWebインターフェイスを提供します。 ファイルのアップロードが完了し、POST要求がAEMサーブレットに送信されます。 このサーブレットは、ドキュメントを抽出し、OutputServiceのgeneratePDFOutputBatchメソッドを呼び出します。 生成されたPDFはzipファイルに圧縮され、エンドユーザーがWebブラウザーからダウンロードできるようになります。

## サーブレットコード{#servlet-code}

サーブレットからのコードスニペットを次に示します。 コードは、要求からテンプレート(xdp)とデータファイル(xml)を抽出します。 テンプレートファイルはファイルシステムに保存されます。 2つのマップが作成されます。templateMapとdataFileMapには、それぞれテンプレートとxml(data)ファイルが含まれます。 次に、DocumentServicesサービスのgenerateMultipleRecordsメソッドが呼び出されます。

```java
for (final java.util.Map.Entry < String, org.apache.sling.api.request.RequestParameter[] > pairs: params
.entrySet()) {
final String key = pairs.getKey();
final org.apache.sling.api.request.RequestParameter[] pArr = pairs.getValue();
final org.apache.sling.api.request.RequestParameter param = pArr[0];
try {
if (!param.isFormField()) {

if (param.getFileName().endsWith("xdp")) {
    final InputStream xdpStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xdpDocument = new com.adobe.aemfd.docmanager.Document(xdpStream);

    xdpDocument.copyToFile(new File(saveLocation + File.separator + "fromui.xdp"));
    templateMap.put("key1", "file://///" + saveLocation + File.separator + "fromui.xdp");
    System.out.println("####  " + param.getFileName());

}
if (param.getFileName().endsWith("xml")) {
    final InputStream xmlStream = param.getInputStream();
    com.adobe.aemfd.docmanager.Document xmlDocument = new com.adobe.aemfd.docmanager.Document(xmlStream);
    dataFileMap.put("key1", xmlDocument);
}
}

Document zippedDocument = documentServices.generateMultiplePdfs(templateMap, dataFileMap,saveLocation);
.....
.....
....
```

### インターフェイス実装コード{#Interface-Implementation-Code}

次のコードは、OutputServiceのgeneratePDFOutputBatchを使用して複数のPDFを生成し、そのPDFファイルを含むzipファイルを呼び出し側サーブレットに返します

```java
public Document generateMultiplePdfs(HashMap < String, String > templateMap, HashMap < String, Document > dataFileMap, String saveLocation) {
    log.debug("will save generated documents to " + saveLocation);
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    com.adobe.fd.output.api.BatchOptions batchOptions = new com.adobe.fd.output.api.BatchOptions();
    batchOptions.setGenerateManyFiles(true);
    com.adobe.fd.output.api.BatchResult batchResult = null;
    try {
        batchResult = outputService.generatePDFOutputBatch(templateMap, dataFileMap, pdfOptions, batchOptions);
        FileOutputStream fos = new FileOutputStream(saveLocation + File.separator + "zippedfile.zip");
        ZipOutputStream zipOut = new ZipOutputStream(fos);
        FileInputStream fis = null;

        for (int i = 0; i < batchResult.getGeneratedDocs().size(); i++) {
              com.adobe.aemfd.docmanager.Document dataMergedDoc = batchResult.getGeneratedDocs().get(i);
            log.debug("Got document " + i);
            dataMergedDoc.copyToFile(new File(saveLocation + File.separator + i + ".pdf"));
            log.debug("saved file " + i);
            File fileToZip = new File(saveLocation + File.separator + i + ".pdf");
            fis = new FileInputStream(fileToZip);
            ZipEntry zipEntry = new ZipEntry(fileToZip.getName());
            zipOut.putNextEntry(zipEntry);
            byte[] bytes = new byte[1024];
            int length;
            while ((length = fis.read(bytes)) >= 0) {
                zipOut.write(bytes, 0, length);
            }
            fis.close();
        }
        zipOut.close();
        fos.close();
        Document zippedDocument = new Document(new File(saveLocation + File.separator + "zippedfile.zip"));
        log.debug("Got zipped file from file system");
        return zippedDocument;


    } catch (OutputServiceException | IOException e) {

        e.printStackTrace();
    }
    return null;


}
```

### サーバーに展開{#Deploy-on-your-server}

ご使用のサーバーでこの機能をテストするには、次の手順に従ってください。

* [zipファイルの内容をダウンロードしてファイルシステムに抽出します](assets/mult-records-template-and-xml-file.zip)。このzipファイルには、テンプレートとxmlデータファイルが含まれています。
* [ブラウザーでFelix Webコンソールを指定](http://localhost:4502/system/console/bundles)
* [DevelopingWithServiceUser Bundleをデプロイします](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)。
* [OutputService APIを使用してPDFを生成するカスタムAEMFormsDocumentServices Bundle](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).Customバンドルのデプロイ
* [ブラウザーでパッケージマネージャーを指定](http://localhost:4502/crx/packmgr/index.jsp)
* [パッケージを読み込んでインストールします](assets/generate-multiple-pdf-from-xml.zip)。このパッケージには、テンプレートとデータファイルをドロップできるhtmlページが含まれています。
* [ブラウザーがMultiRecords.htmlを指すように指定します。](http://localhost:4502/content/DocumentServices/Multirecord.html?)
* テンプレートとxmlデータファイルを一緒にドラッグ&amp;ドロップします
* 作成したzipファイルをダウンロードします。 このzipファイルには、Outputサービスで生成されたpdfファイルが含まれています。

>[!NOTE]
>この機能をトリガーする方法は複数あります。 この例では、Webインターフェイスを使用してテンプレートとデータファイルをドロップし、この機能を示しています。

