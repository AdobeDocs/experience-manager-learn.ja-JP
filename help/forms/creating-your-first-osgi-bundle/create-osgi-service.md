---
title: AEM Formsでの最初のOSGiサービスの作成
description: 'AEM Formsでの最初のOSGiサービスの構築 '
feature: アダプティブフォーム
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 5%

---


# OSGiサービス

OSGiサービスは、Javaクラスまたはサービスインターフェイスで、名前と値のペアとして多数のサービスプロパティを含みます。 サービスプロパティは、同じサービスインターフェイスでサービスを提供する異なるサービスプロバイダ間を区別します。

OSGiサービスは、サービスインターフェイスによって意味的に定義され、サービスオブジェクトとして実装されます。 サービスの機能は、実装するインターフェイスによって定義されます。 したがって、異なるアプリケーションが同じサービスを実装できます。 サービスインターフェイスを使用すると、実装ではなく、インターフェイスをバインディングしてバンドルを操作できます。 サービスインターフェイスは、できるだけ少ない実装の詳細で指定する必要があります。

## インターフェイスの定義

<span class="x x-first x-last">XDP</span>テンプレートとデータを結合する1つのメソッドを備えたシンプルなインターフェイスです。

```java
package com.learningaemforms.adobe.core;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface {
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
} 
```

## インターフェイスの実装

インターフェイスの実装を保持する`com.learningaemforms.adobe.core.impl`という新しいパッケージを作成します。

```java
package com.learningaemforms.adobe.core.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.learningaemforms.adobe.core.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

10行目の注釈`@Component(...)`は、このJavaクラスをOSGiコンポーネントとしてマークし、OSGiサービスとして登録します。

`@Reference`注釈はOSGi宣言サービスの一部で、[Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html)の参照を変数`outputService`に挿入するために使用されます。


## バンドルのビルドとデプロイ

* **コマンドプロンプトウィンドウ**&#x200B;を開きます。
* `c:\aemformsbundles\learningaemforms\core` に移動します。
* コマンド`mvn clean install -PautoInstallBundle`を実行します。
* 上記のコマンドは、localhost:4502で実行されているAEMインスタンスにバンドルを自動的にビルドしてデプロイします。

バンドルは、次の場所`C:\AEMFormsBundles\learningaemforms\core\target`でも使用できます。 バンドルは、[Felix Webコンソールを使用してAEMにデプロイすることもできます。](http://localhost:4502/system/console/bundles)

## サービスの使用

これで、JSPページでサービスを使用できます。 次のコードスニペットは、サービスへのアクセス権を取得し、サービスによって実装されたメソッドを使用する方法を示しています

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.learningaemforms.adobe.core.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

JSPページを含むサンプルパッケージは、![こちらからダウンロードできます。](assets/learning-aem-forms.zip)

## パッケージのテスト

[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して、パッケージをAEMに読み込んでインストールします。

postmanを使用してPOST呼び出しを行い、下のスクリーンショットに示すように入力パラメーターを指定します。
![postman](assets/test-service-postman.JPG)
