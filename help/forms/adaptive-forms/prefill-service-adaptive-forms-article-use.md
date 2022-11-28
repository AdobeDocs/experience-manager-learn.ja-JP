---
title: アダプティブFormsでの事前入力サービス
description: バックエンドのデータソースからデータを取得して、アダプティブフォームに事前入力する。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: 381812397fa7d15f6ee34ef85ddf0aa0acc0af42
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 9%

---

# アダプティブFormsでの事前入力サービスの使用

既存データを使用して、アダプティブフォームのフィールドを事前入力することができます。ユーザーがフォームを開くと、これらのフィールドの値は事前入力されています。アダプティブフォームのフィールドに事前入力する方法は複数あります。 この記事では、AEM Forms事前入力サービスを使用したアダプティブフォームの事前入力について説明します。

アダプティブフォームの事前入力方法の詳細については、以下を参照してください。 [このドキュメントに従う](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

事前入力サービスを使用してアダプティブフォームに事前入力するには、 `com.adobe.forms.common.service.DataXMLProvider` インターフェイス。 メソッド `getDataXMLForDataRef` は、アダプティブフォームがフィールドの事前入力に使用するデータを作成して返すためのロジックを持ちます。 このメソッドでは、任意のソースからデータを取得し、データドキュメントの入力ストリームを返すことができます。 以下のサンプルコードは、ログインしているユーザーのユーザープロファイル情報を取得し、XML ドキュメントを作成します。この XML ドキュメントの入力ストリームは、アダプティブフォームで使用されます。

以下のコードスニペットには、DataXMLProvider インターフェイスを実装するクラスがあります。 ログインしたユーザーにアクセスし、ログインしたユーザーのプロファイル情報を取得します。 次に、「data」というルートノード要素を持つ XML ドキュメントを作成し、このデータノードに適切な要素を追加します。 XML ドキュメントが構築されると、XML ドキュメントの入力ストリームが返されます。

次に、このクラスは OSGi バンドルになり、AEMにデプロイされます。 バンドルがデプロイされると、この事前入力サービスをアダプティブフォームの事前入力サービスとして使用できるようになります。

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

ご使用のサーバーでこの機能をテストするには、次の手順を実行してください

* ログインした [ユーザーのプロフィール](http://localhost:4502/security/users.html) 情報が入力されます。 この例では、ログインしたユーザーの FirstName、LastName、Email の各プロパティを探します。
* [zip ファイルの内容をダウンロードし、コンピューターに展開します。](assets/prefillservice.zip)
* prefill.core-1.0.0-SNAPSHOT バンドルをデプロイするには、 [AEM web コンソール](http://localhost:4502/system/console/bundles)
* 「作成」タブを使用してアダプティブフォームを読み込む |からのファイルのアップロード [FormsAndDocuments セクション](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 次を確認します。 [フォーム](http://localhost:4502/editor.html/content/forms/af/prefill.html) が次を使用している **&quot;カスタムAEM Forms事前入力サービス&quot;** を事前入力サービスとして使用します。 これは、 **フォームコンテナ** 」セクションに入力します。
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). フォームに正しい値が入力されているのが確認できます。

>[!NOTE]
>
>com.aem.prefill.core.PrefillAdaptiveForm のデバッグを有効にしている場合、AEMサーバーのインストールフォルダーに書き込まれた、生成された xml データファイル。

