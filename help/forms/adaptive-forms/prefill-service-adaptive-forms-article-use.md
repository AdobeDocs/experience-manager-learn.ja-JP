---
title: アダプティブFormsでの事前入力サービス
seo-title: アダプティブFormsでの事前入力サービス
description: バックエンドデータソースからデータを取得して、アダプティブフォームに事前入力する。
seo-description: バックエンドデータソースからデータを取得して、アダプティブフォームに事前入力する。
sub-product: フォーム[ふぉーむ]
feature: アダプティブフォーム
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
topic: 開発
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 7%

---


# アダプティブFormsでの事前入力サービスの使用

既存データを使用して、アダプティブフォームのフィールドを事前入力することができます。ユーザーがフォームを開くと、これらのフィールドの値は事前入力されています。アダプティブフォームフィールドの事前入力方法は複数あります。 この記事では、AEM Forms事前入力サービスを使用したアダプティブフォームの事前入力について説明します。

アダプティブフォームの事前入力方法の詳細については、[このドキュメント](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)を参照してください。

事前入力サービスを使用してアダプティブフォームに事前入力するには、DataProviderインターフェイスを実装するクラスを作成する必要があります。 getPrefillDataメソッドは、アダプティブフォームがフィールドの事前入力のために使用するデータを作成して返すロジックを持ちます。 このメソッドでは、任意のソースからデータを取得し、データドキュメントの入力ストリームを返すことができます。 以下のサンプルコードは、ログインしたユーザーのユーザープロファイル情報を取得し、XMLドキュメントを作成します。このXMLドキュメントの入力ストリームは、アダプティブフォームで使用されるように返されます。

以下のコードスニペットには、DataProviderインターフェイスを実装するクラスがあります。 ログインしたユーザーにアクセスし、ログインしたユーザーのプロファイル情報を取得します。 次に、「data」というルートノード要素を持つXMLドキュメントを作成し、このデータノードに適切な要素を追加します。 XMLドキュメントが構築されると、XMLドキュメントの入力ストリームが返されます。

次に、このクラスはOSGiバンドルになり、AEMにデプロイされます。 バンドルがデプロイされると、この事前入力サービスをアダプティブフォームの事前入力サービスとして使用できるようになります。

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

この機能をサーバーでテストするには、次の手順を実行します

* [zipファイルの内容をダウンロードし、コンピューターに抽出します。](assets/prefillservice.zip)
* ログインした[ユーザーのプロファイル](http://localhost:4502/libs/granite/security/content/useradmin)情報が完全に入力されていることを確認します。 これは、サンプルが動作するために必要です。 このサンプルには、見つからないユーザープロファイルプロパティのエラーチェックはありません。
* [AEM Webコンソール](http://localhost:4502/system/console/bundles)を使用してバンドルをデプロイします。
* XSDを使用したアダプティブフォームの作成
* アダプティブフォームの事前入力サービスとして「Custom Aem Forms Pre Fill Service」を関連付ける
* スキーマ要素をフォームにドラッグ&amp;ドロップします
* フォームをプレビューする

>[!NOTE]
>
>アダプティブフォームがXSDに基づいている場合は、事前入力サービスによって返されるXMLドキュメントが、アダプティブフォームのXSDに基づいているものと一致していることを確認します。
>
>アダプティブフォームがXSDに基づいていない場合は、フィールドを手動でバインドする必要があります。 例えば、アダプティブフォームフィールドをXMLデータ内のfname要素にバインドするには、アダプティブフォームフィールドのバインド参照で`/data/fname`を使用します。

