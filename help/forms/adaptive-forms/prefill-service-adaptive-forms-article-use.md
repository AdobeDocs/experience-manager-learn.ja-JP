---
title: アダプティブFormsでの事前入力サービス
description: バックエンドのデータソースからデータを取得して、アダプティブフォームに事前入力する。
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 7%

---

# アダプティブFormsでの事前入力サービスの使用

既存データを使用して、アダプティブフォームのフィールドを事前入力することができます。ユーザーがフォームを開くと、これらのフィールドの値は事前入力されています。アダプティブフォームのフィールドに事前入力する方法は複数あります。 この記事では、AEM Forms事前入力サービスを使用したアダプティブフォームの事前入力について説明します。

アダプティブフォームの事前入力方法の詳細については、以下を参照してください。 [このドキュメントに従う](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

事前入力サービスを使用してアダプティブフォームに事前入力するには、DataProvider インターフェイスを実装するクラスを作成する必要があります。 getPrefillData メソッドは、アダプティブフォームがフィールドの事前入力に使用するデータを作成して返すロジックを持ちます。 このメソッドでは、任意のソースからデータを取得し、データドキュメントの入力ストリームを返すことができます。 以下のサンプルコードは、ログインしたユーザーのユーザープロファイル情報を取得し、XML ドキュメントを作成します。この XML ドキュメントの入力ストリームは、アダプティブフォームで使用されます。

以下のコードスニペットには、DataProvider インターフェイスを実装するクラスがあります。 ログインしたユーザーにアクセスし、ログインしたユーザーのプロファイル情報を取得します。 次に、「data」というルートノード要素を持つ XML ドキュメントを作成し、このデータノードに適切な要素を追加します。 XML ドキュメントが構築されると、XML ドキュメントの入力ストリームが返されます。

次に、このクラスは OSGi バンドルになり、AEMにデプロイされます。 バンドルがデプロイされると、この事前入力サービスをアダプティブフォームの事前入力サービスとして使用できるようになります。

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

ご使用のサーバーでこの機能をテストするには、次の手順を実行してください

* [zip ファイルの内容をダウンロードし、コンピューターに展開します。](assets/prefillservice.zip)
* ログインした [ユーザーのプロフィール](http://localhost:4502/libs/granite/security/content/useradmin) 情報が完全に入力されます。 これは、サンプルが動作するために必要です。 このサンプルには、見つからないユーザープロファイルプロパティを確認するエラーはありません。
* を使用してバンドルをデプロイします。 [AEM web コンソール](http://localhost:4502/system/console/bundles)
* XSD を使用してアダプティブフォームを作成する
* 「カスタム Aem Form 事前入力サービス」をアダプティブフォームの事前入力サービスとして関連付ける
* スキーマ要素をフォームにドラッグ&amp;ドロップします
* フォームをプレビューする

>[!NOTE]
>
>アダプティブフォームが XSD に基づいている場合は、事前入力サービスによって返される XML ドキュメントが、アダプティブフォームの基になっている XSD と一致していることを確認します。
>
>アダプティブフォームが XSD に基づいていない場合は、これらのフィールドを手動でバインドする必要があります。 例えば、使用する XML データ内の FNAME 要素にアダプティブフォームフィールドをバインドする場合 `/data/fname`  アダプティブフォームフィールドのバインド参照を参照してください。
