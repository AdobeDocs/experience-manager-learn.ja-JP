---
title: AEM FormsでのAdobe Sign API の使用
description: Adobe Sign ヘルパーメソッドを使用した署名用ドキュメントの送信
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: 81026b569ae0dc9976f714715682448a41d9f8bc
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# Adobe Sign ヘルパーメソッドの使用

ユースケースによっては、AEM ワークフローを使用せずに署名用のドキュメントを送信する必要が生じる場合があります。 このような場合、この記事で提供されるサンプルバンドルで公開されているラッパーメソッドを使用すると非常に便利です。

## サンプル OSGi バンドルのデプロイ

[OSGi バンドルのデプロイ](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) AEM OSGi web コンソールを使用します。 AEM OSGi web コンソールの Configuration Manager で、API 統合キーと API ユーザーを、以下に示すように OSGi 設定を使用して指定します。

 に注意してください `AdobeSignHelperMethods` OSGi バンドルは、Adobe Experience Manager（AEM）Adobeコードとして認識されないので、製品サポートではサポートされません。
![sign-configuration](assets/sign-configuration.png)


## API ドキュメント

経由で以下を利用できます `AcrobatSignHelperMethods` OSGi バンドルで提供される OSGi サービス。

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


契約書または Web フォームの作成に使用されるドキュメントです。 ドキュメントは、送信者によって最初にAcrobat Signにアップロードされます。 これを、以下と呼びます _一時的_ これは、アップロード後 7 日間のみ使用できるので、 このメソッドはを受け入れます `com.adobe.aemfd.docmanager.Document` 一時的なドキュメント ID を返します。

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

メールパラメーターで識別されるユーザーに署名するために、一時的なドキュメント ID を使用して署名用のドキュメントを送信します。

### getWidgetID

`String getWidgetID(String transientDocumentID)`

ウィジェットは、ユーザーに複数回提示し、複数回署名できる再利用可能なテンプレートのようなものです。 このメソッドを使用して、一時的なドキュメント ID を使用してウィジェット ID を取得します。

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

特定のウィジェット ID のウィジェット URL を取得します。 このウィジェット URL は、ドキュメントに署名するユーザーに表示できます。

## API の使用

この `AcrobatSignHelperMethods` は OSGi サービスなので、java コードで@Reference 注釈を使用して注釈を付ける必要があります。

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```

