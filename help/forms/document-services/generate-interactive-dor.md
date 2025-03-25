---
title: アダプティブフォームデータを使用してインタラクティブ DoR を生成する
description: アダプティブフォームデータを XDP テンプレートと結合してダウンロード可能な PDF を生成する
version: Experience Manager 6.4, Experience Manager 6.5
feature: Forms Service
topic: Development
role: Developer
level: Experienced
jira: KT-9226
exl-id: d9618cc8-d399-4850-8714-c38991862045
last-substantial-update: 2020-02-07T00:00:00Z
duration: 177
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 100%

---

# インタラクティブ DoR をダウンロードする

一般的な使用例としては、アダプティブフォームデータを含むインタラクティブ DoR をダウンロードできる場合が考えられます。 ダウンロードした DoR は、Adobe Acrobat または Adobe Reader を使用して完了します。

## アダプティブフォームが XSD スキーマに基づいていない

XDP とアダプティブフォームがどのスキーマにも基づいていない場合は、次の手順に従ってインタラクティブなレコードのドキュメントを生成してください。

### アダプティブフォームの作成

アダプティブフォームを作成し、アダプティブフォームのフィールド名が xdp テンプレートのフィールド名と同じ名前になっていることを確認します。
xdp テンプレートのルート要素名をメモしておきます。
![root-element](assets/xfa-root-element.png)

### クライアントライブラリ

次のコードは、「PDF をダウンロード」ボタンがトリガーされるとトリガーされます

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","two.xdp")
                postParameters.append("formBasedOnSchema", "false");
                postParameters.append("xfaRootElement","form1");
                console.log(guideResultObject.data);
                req.send(postParameters);
                req.onreadystatechange = function() {
                    if (req.readyState == 4 && req.status == 200) {
                        download(this.response, "report.pdf", "application/pdf");
                    }


                }
            }
        });

    });
});
```

## XSD スキーマに基づくアダプティブフォーム

xdp が XSD に基づいていない場合は、次の手順に従ってアダプティブフォームのベースとなる XSD（スキーマ）を作成します

### XDP のサンプルデータを生成

* XDP を AEM Forms Designer で開きます。
* ファイル／フォームのプロパティ／プレビューを選択します。
* 「プレビューデータを生成」をクリックします
* 「生成」をクリックします
* 「form-data.xml」などの意味のあるファイル名を指定します

### xml データから XSD を生成

無料のオンラインツールを使用して、前の手順で生成した xml データから [XSD を生成](https://www.freeformatter.com/xsd-generator.html?lang=ja)できます。

### アダプティブフォームの作成

前の手順の XSD に基づいてアダプティブフォームを作成します。クライアントライブラリ「irs」を使用するようにフォームを関連付けます。このクライアントライブラリには、呼び出し元のアプリケーションに PDF を返すサーブレットに対して POST 呼び出しを行うコードが含まれています。
次のコードは、「_PDF をダウンロード_」をクリックするとトリガーされます

```javascript
$(document).ready(function() {
    $(".downloadPDF").click(function() {
        window.guideBridge.getDataXML({
            success: function(guideResultObject) {
                var req = new XMLHttpRequest();
                req.open("POST", "/bin/generateinteractivedor", true);
                req.responseType = "blob";
                var postParameters = new FormData();
                postParameters.append("dataXml", guideResultObject.data);
                postParameters.append("xdpName","f8918-r14e_redo-barcode_3 2.xdp")
                postParameters.append("formBasedOnSchema", "true");
                postParameters.append("dataNodeToExtract","afData/afBoundData/topmostSubform");
                console.log(guideResultObject.data);
                req.send(postParameters);
                req.onreadystatechange = function() {
                    if (req.readyState == 4 && req.status == 200) {
                        download(this.response, "report.pdf", "application/pdf");
                    }


                }
            }
        });

    });
});
```



## カスタムサーブレットを作成

データを XDP テンプレートと結合し、PDF を返すカスタムサーブレットを作成します。 これを行うコードを以下に示します。カスタムサーブレットは、[AEMFormsDocumentServices.core-1.0-SNAPSHOT バンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)の一部です）。

```java
public class GenerateIInteractiveDor extends SlingAllMethodsServlet {
    private static final long serialVersionUID = 1 L;
    @Reference
    DocumentServices documentServices;
    @Reference
    FormsService formsService;
    private static final Logger log = LoggerFactory.getLogger(GenerateIInteractiveDor.class);

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        doPost(request, response);
    }
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        String xdpName = request.getParameter("xdpName");

        boolean formBasedOnXSD = Boolean.parseBoolean(request.getParameter("formBasedOnSchema"));

        XPathFactory xfact = XPathFactory.newInstance();
        XPath xpath = xfact.newXPath();
        String dataXml = request.getParameter("dataXml");
        log.debug("The data xml is " + dataXml);
        org.w3c.dom.Document xmlDataDoc = documentServices.w3cDocumentFromStrng(dataXml);
        Document renderedPDF = null;
        try {
            if (!formBasedOnXSD) {
                String xfaRootElement = request.getParameter("xfaRootElement");
                DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
                DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
                org.w3c.dom.Document newXMLDocument = dBuilder.newDocument();
                Element rootElement = newXMLDocument.createElement(xfaRootElement);
                String unboundData = "afData/afUnboundData/data";
                Node dataNode = (Node) xpath.evaluate(unboundData, xmlDataDoc, XPathConstants.NODE);
                NodeList dataChildNodes = dataNode.getChildNodes();
                for (int i = 0; i<dataChildNodes.getLength(); i++) {
                    Node childNode = dataChildNodes.item(i);
                    if (childNode.getNodeType() == 1) {
                        Element newElement = newXMLDocument.createElement(childNode.getNodeName());
                        newElement.setTextContent(childNode.getTextContent());
                        rootElement.appendChild(newElement);
                        log.debug("the node name is  " + childNode.getNodeName() + " and its value is " + childNode.getTextContent());
                    }
                }
                newXMLDocument.appendChild(rootElement);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(newXMLDocument);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);

            } else {
                // form is based on xsd
                // get the actual xml data that needs to be merged with the template. This can be made more generic
                String nodeToExtract = request.getParameter("dataNodeToExtract");
                Node dataNode = (Node) xpath.evaluate(nodeToExtract, xmlDataDoc, XPathConstants.NODE);
                StringWriter writer = new StringWriter();
                Transformer transformer = TransformerFactory.newInstance().newTransformer();
                transformer.transform(new DOMSource(dataNode), new StreamResult(writer));
                String xml = writer.toString();
                System.out.println(xml);
                xmlDataDoc = documentServices.w3cDocumentFromStrng(xml);
                Document xmlDataDocument = documentServices.orgw3cDocumentToAEMFDDocument(xmlDataDoc);
                String xdpTemplatePath = "crx:///content/dam/formsanddocuments";
                com.adobe.fd.forms.api.PDFFormRenderOptions renderOptions = new com.adobe.fd.forms.api.PDFFormRenderOptions();
                renderOptions.setAcrobatVersion(com.adobe.fd.forms.api.AcrobatVersion.Acrobat_11);
                renderOptions.setContentRoot(xdpTemplatePath);
                renderOptions.setRenderAtClient(com.adobe.fd.forms.api.RenderAtClient.NO);
                renderedPDF = formsService.renderPDFForm(xdpName, xmlDataDocument, renderOptions);
            }
            InputStream fileInputStream = renderedPDF.getInputStream();
            response.setContentType("application/pdf");
            response.addHeader("Content-Disposition", "attachment; filename=" + xdpName.replace("xdp", "pdf"));
            response.setContentLength((int) fileInputStream.available());
            OutputStream responseOutputStream = response.getOutputStream();
            int bytes;
            while ((bytes = fileInputStream.read()) != -1) {
                responseOutputStream.write(bytes);
            }
            responseOutputStream.flush();
            responseOutputStream.close();

        } catch (XPathExpressionException | TransformerException | FormsServiceException | IOException | ParserConfigurationException e) {
            log.debug(e.getMessage());
        }

    }

}
```

このサンプルコードでは、リクエストオブジェクトから XDP 名と他のパラメーターが抽出されます。フォームが XSD に基づいていない場合は、XDP と結合する新しい XML ドキュメントが作成されます。ただし、フォームが XSD ベースの場合、関連するノードはアダプティブフォームの送信済みデータから直接抽出され、それに応じて XDP テンプレートと結合する XML ドキュメントが生成されます。

## サンプルのサーバーへのデプロイ

ローカルサーバーでこれをテストするには、次の手順に従います。

1. [DevelopingWithServiceUser バンドルをダウンロードしてインストールします](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
1. Apache Sling Service User Mapper Service に次のエントリを追加します。
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
1. [カスタム DocumentServices バンドルをダウンロードしてインストールします](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)。これには、データと XDP テンプレートを結合し、PDF をストリーミングして戻すサーブレットがあります
1. [クライアントライブラリを読み込みます。](assets/generate-interactive-dor-client-lib.zip)
1. [記事アセット（アダプティブフォーム、XDP テンプレートおよび XSD）の読み込み](assets/generate-interactive-dor-sample-assets.zip)
1. [アダプティブフォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/f8918complete/jcr:content?wcmmode=disabled)
1. いくつかのフォームフィールドに入力します。
1. 「PDF をダウンロード」をクリックして PDF を取得します。PDF のダウンロードが完了するまで、数秒待つ必要がある場合があります。

>[!NOTE]
>
>ダウンロードした PDF をブラウザーの PDF ビューアを使用して開くと、PDF のデータは表示されません。ダウンロードした PDF は、Adobe Acrobat または Adobe Reader を使用して開きます。


>[!NOTE]
>
>[非 xsd ベースのアダプティブフォーム](http://localhost:4502/content/dam/formsanddocuments/two/jcr:content?wcmmode=disabled)でも同じ使用例を試すことができます。irs クライアントライブラリにある streampdf.js の POST エンドポイントに必ず適切なパラメーターを渡してください。
