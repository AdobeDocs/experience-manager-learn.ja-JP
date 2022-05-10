---
title: DAM でのAEM Forms DoR のタグ付けと保存
description: この記事では、AEM DAM でAEM Formsによって生成された DoR の保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいておこなわれます。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 832f04b4-f22f-4cf9-8136-e3c1081de7a9
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '612'
ht-degree: 1%

---

# DAM でのAEM Forms DoR のタグ付けと保存 {#tagging-and-storing-aem-forms-dor-in-dam}

この記事では、AEM DAM でAEM Formsによって生成された DoR の保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいておこなわれます。

よくあるお客様からの質問は、AEM DAM でAEM Formsによって生成されたレコードのドキュメント (DoR) を保存し、タグ付けすることです。 ドキュメントのタグ付けは、アダプティブFormsの送信済みデータに基づいておこなう必要があります。 例えば、送信されたデータの雇用状況が「退職」の場合、ドキュメントに「退職」タグを付け、ドキュメントを DAM に保存します。

使用例を次に示します。

* ユーザーがアダプティブフォームに入力します。 アダプティブフォームでは、ユーザーの配偶者の有無（例：独身）と雇用ステータス（例：退職）が取り込まれます。
* フォームの送信時に、AEM Workflow がトリガーされます。 婚姻区分（独身）と雇用区分（退職）をドキュメントにタグ付けし、ドキュメントを DAM に保存します。
* ドキュメントが DAM に保存されると、管理者はこれらのタグでドキュメントを検索できるようになります。 例えば、「Single」または「Retired」の検索では、適切な DoR が取得されます。

この使用例を満たすために、カスタムプロセスステップが記述されました。 この手順では、送信されたデータから適切なデータ要素の値を取得します。 次に、この値を使用してタグタイルを作成します。 例えば、婚姻区分要素の値が「Single」の場合、タグのタイトルは**Peak:EmploymentStatus/Single になります。 ** TagManager API を使用して、タグを見つけ、DoR にタグを適用します。

以下は、タグ付けしてレコードのドキュメントをAEM DAM に保存するための完全なコードです。

```java
package com.aemforms.setvalue.core;
import java.io.InputStream;
import javax.jcr.Node;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.xpath.XPath;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowData;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.adobe.granite.workflow.model.WorkflowModel;
import com.day.cq.tagging.Tag;
import com.day.cq.tagging.TagManager;

@Component(property = {
   Constants.SERVICE_DESCRIPTION + "=Tag and Store Dor in DAM",
   Constants.SERVICE_VENDOR + "=Adobe Systems",
   "process.label" + "=Tag and Store Dor in DAM"
})
@Designate(ocd = TagDorServiceConfiguration.class)
public class TagAndStoreDoRinDAM implements WorkflowProcess
{
   private static final Logger log = LoggerFactory.getLogger(TagAndStoreDoRinDAM.class);

   private TagDorServiceConfiguration serviceConfig;
   @Activate
   public void activate(TagDorServiceConfiguration config)
   {
      this.serviceConfig = config;
   }
   @Override
   public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap arg2) throws WorkflowException
   {
       log.debug("The process arguments passed ..." + arg2.get("PROCESS_ARGS", "string").toString());
      String params = arg2.get("PROCESS_ARGS", "string").toString();
      WorkflowModel wfModel = workflowSession.getModel("/var/workflow/models/dam/update_asset");
      // Read the Tag DoR service configuration
      String damFolder = serviceConfig.damFolder();
      String dorPDFName = serviceConfig.dorPath();
      String dataXmlFile = serviceConfig.dataFilePath();
      log.debug("The Data Xml File is ..." + dataXmlFile + "DorPDFName" + dorPDFName);
      // Read the arguments passed to this workflow step
      String parameters[] = params.split(",");
      log.debug("The %%%% length of parameters is " + parameters.length);
      Tag[] tagArray = new Tag[parameters.length];
      WorkflowData wfData = workItem.getWorkflowData();
      String dorFileName = (String) wfData.getMetaDataMap().get("filename");
      log.debug("The dorFileName is ..." + dorFileName);
      String payloadPath = workItem.getWorkflowData().getPayload().toString();
      String dataFilePath = payloadPath + "/" + dataXmlFile + "/jcr:content";
      String dorDocumentPath = payloadPath + "/" + dorPDFName + "/jcr:content";
      log.debug("Data File Path" + dataFilePath);
      log.debug("Dor File Path" + dorDocumentPath);
      Session session = workflowSession.adaptTo(Session.class);
      ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
      com.day.cq.dam.api.AssetManager assetMgr = resourceResolver.adaptTo(com.day.cq.dam.api.AssetManager.class);
      DocumentBuilderFactory factory = null;
      DocumentBuilder builder = null;
      Document xmlDocument = null;
      Node xmlDataNode = null;
      Node dorDocumentNode = null;

      try
      {
         // create org.w3c.dom.Document object from submitted form data
         xmlDataNode = session.getNode(dataFilePath);
         log.debug("xml Data Node" + xmlDataNode.getName());
         dorDocumentNode = session.getNode(dorDocumentPath);
         log.debug("DOR Document Node is " + dorDocumentNode.getName());
         InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
         InputStream dorInputStream = dorDocumentNode.getProperty("jcr:data").getBinary().getStream();
         XPath xPath = javax.xml.xpath.XPathFactory.newInstance().newXPath();
         factory = DocumentBuilderFactory.newInstance();
         builder = factory.newDocumentBuilder();
         xmlDocument = builder.parse(xmlDataStream);
         String newFile = "/content/dam/" + damFolder + "/" + dorFileName;
         log.debug("the new file is ..." + newFile);
         // Store the DoR in DAM
         assetMgr.createAsset(newFile, dorInputStream, "application/pdf", true);
         WorkflowData wfDataLoad = workflowSession.newWorkflowData("JCR_PATH", newFile);
         log.debug("Wrote the document to DAM" + newFile);
         TagManager tagManager = resourceResolver.adaptTo(TagManager.class);
         Resource pdfDocumentNode = resourceResolver.getResource(newFile);
         Resource metadata = pdfDocumentNode.getChild("jcr:content/metadata");
         // Fetch the xml elements from the xml document
         for (int i = 0; i < parameters.length; i++)
            {
                String tagTitle = parameters[i].split("=")[0];
                log.debug("The tag title is" + tagTitle);
                String nameOfNode = parameters[i].split("=")[1];
                org.w3c.dom.Node xmlElement = (org.w3c.dom.Node) xPath.compile(nameOfNode).evaluate(xmlDocument, javax.xml.xpath.XPathConstants.NODE);
                log.debug("###The value data node is " + xmlElement.getTextContent());
                Tag tagFound = tagManager.resolveByTitle(tagTitle + xmlElement.getTextContent());
                log.debug("The tag found was ..." + tagFound.getPath());
                tagArray[i] = tagFound;
            }
         tagManager.setTags(metadata, tagArray, true);
         workflowSession.startWorkflow(wfModel, wfDataLoad);
         log.debug("Workflow started");
         log.debug("Done setting tags");
         xmlDataStream.close();
         dorInputStream.close();
      } catch (Exception e)
            {
                 log.debug("The error message is " + e.getMessage());
            }

   }

}
```

このサンプルをシステムで動作させるには、次の手順に従ってください。
* [Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalue バンドルをダウンロードしてデプロイします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). 送信されたフォームデータからタグを設定するカスタム OSGi バンドルです。

* [サンプルのアダプティブフォームのダウンロード](assets/tag-and-store-in-dam-assets.zip)

* [Formsとドキュメントに移動](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 「作成」をクリックします。 |ファイルサンプルの adaptiveform.zip をアップロードおよびアップロードします

* [記事アセットを読み込む](assets/tag-and-store-in-dam-assets.zip) AEM package manager の使用
* を開きます。 [プレビューモードでのサンプルフォーム](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). 「人」セクションに入力し、フォームを送信します。
* [DAM の Peak フォルダーに移動します。](http://localhost:4502/assets.html/content/dam/Peak). Peak フォルダーに DoR が表示されます。 ドキュメントのプロパティを確認します。 適切にタグ付けする必要があります。
おめでとう!! サンプルがシステムに正常にインストールされました

* 次に、 [ワークフロー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) フォームの送信時にトリガーされる
* ワークフローの最初の手順では、申請者名と居住国を連結して一意のファイル名を作成します。
* ワークフローの 2 番目の手順では、タグ階層と、タグ付けが必要なフォームフィールド要素を渡します。 プロセスステップは、送信されたデータから値を抽出し、ドキュメントにタグ付けする必要のあるタグタイトルを作成します。
* DoR を DAM の別のフォルダーに保存する場合は、以下のスクリーンショットで指定されている設定プロパティを使用して、フォルダーの場所を指定します。

その他の 2 つのパラメーターは、アダプティブフォーム送信オプションで指定された DoR とデータファイルパスに固有です。 ここで指定した値が、アダプティブフォームの送信オプションで指定した値と一致していることを確認してください。

![タグ Dor](assets/tag_dor_service_configuration.gif)
