---
title: AEM Forms での Adobe Sign API の使用
description: Adobe Sign ヘルパーメソッドを使用して、署名用のドキュメントを送信
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '298'
ht-degree: 100%

---

# Adobe Sign ヘルパーメソッドの使用

特定のユースケースでは、AEM ワークフローを使用せずに署名用のドキュメントを送信することが必要になる場合があります。このような場合、この記事の説明に従って、サンプルバンドルで公開されているラッパーメソッドを使用すると非常に便利です。

## サンプル OSGi バンドルのデプロイ

AEM OSGi web コンソール経由で [OSGi バンドルをデプロイ](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar)します。AEM OSGi web コンソールの設定マネージャーから、以下に示すように OSGi 設定を使用して API 統合キーと API ユーザーを指定します。

`AdobeSignHelperMethods` OSGi バンドルは Adobe Experience Manager（AEM）製品コードとして認識されないので、アドビサポートではサポートされません。
![sign-configuration](assets/sign-configuration.png)


## API に関するドキュメント

次は、OSGi バンドルで提供される `AcrobatSignHelperMethods` OSGi サービスを通じて使用できます。

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


契約書または web フォームの作成に使用されるドキュメントです。ドキュメントは、まず送信者によって Acrobat Sign にアップロードされます。これは、アップロード後 7 日間のみ使用できるので、_一時的なドキュメント_&#x200B;と呼ばれます。このメソッドは、`com.adobe.aemfd.docmanager.Document` を受け入れ、一時的なドキュメント ID を返します。

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

署名用の一時的なドキュメント ID を使用して、メールパラメーターで識別されるユーザーに署名用のドキュメントを送信します。

### getWidgetID

`String getWidgetID(String transientDocumentID)`

ウィジェットは、ユーザーに複数回表示し、複数回署名できる再利用可能なテンプレートのようなものです。このメソッドを使用して、一時的なドキュメント ID を使用してウィジェット ID を取得します。

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

特定のウィジェット ID のウィジェット URL を取得します。このウィジェット URL は、ドキュメントへの署名を目的にユーザーに表示できます。

## API の使用

`AcrobatSignHelperMethods` は OSGi サービスなので、Java コードで @Reference 注釈を使用して注釈を付ける必要があります。

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
