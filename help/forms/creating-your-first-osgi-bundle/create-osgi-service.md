---
title: AEM Formsでの最初の OSGi サービスの作成
description: AEM Formsでの最初の OSGi サービスの構築
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
source-git-commit: f4e86059d29acf402de5242f033a25f913febf36
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 4%

---

# OSGi サービス

OSGi サービスは Java クラスまたはサービスインターフェイスで、多数のサービスプロパティを名前/値のペアとして使用します。 サービスプロパティは、同じサービスインターフェイスでサービスを提供する異なるサービスプロバイダ間で異なります。

OSGi サービスは、サービスインターフェイスによって意味的に定義され、サービスオブジェクトとして実装されます。 サービスの機能は、実装するインターフェイスによって定義されます。 したがって、異なるアプリケーションが同じサービスを実装できます。 サービスインターフェイスを使用すると、バンドルは実装ではなく、インターフェイスをバインドしてやり取りできます。 サービスインターフェイスは、できるだけ少ない実装の詳細で指定する必要があります。

## インターフェイスの定義

データを <span class="x x-first x-last">XDP</span> テンプレート。

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
	public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## インターフェイスの実装

という名前の新しいパッケージを作成します。 `com.mysite.samples.impl` インターフェイスの実装を保持する

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
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

注釈 `@Component(...)` 行 10 では、この Java クラスを OSGi コンポーネントとしてマークし、OSGi サービスとして登録します。

この `@Reference` 注釈は、OSGi 宣言サービスの一部で、 [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) 変数に `outputService`.


## バンドルのビルドとデプロイ

* 開く **コマンドプロンプトウィンドウ**
* `c:\aemformsbundles\mysite\core` に移動します。
* コマンドを実行します。 `mvn clean install -PautoInstallBundle`
* 上記のコマンドは、localhost:4502 で実行されているAEMインスタンスにバンドルを自動的にビルドしてデプロイします。

バンドルは、次の場所でも使用できます `C:\AEMFormsBundles\mysite\core\target`. バンドルは、 [Felix Web コンソール。](http://localhost:4502/system/console/bundles)

## サービスの使用

これで、JSP ページでサービスを使用できます。 次のコードスニペットは、サービスにアクセスし、サービスで実装されたメソッドを使用する方法を示しています

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

JSP ページを含むサンプルパッケージは、次のようになります。 [ここからダウンロード](assets/learning_aem_forms.zip)

[完全なバンドルをダウンロードできます](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## パッケージをテストする

を使用してパッケージをAEMにインポートおよびインストールします。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)

Postman を使用してPOST呼び出しをおこない、以下のスクリーンショットに示すように入力パラメーターを指定します。
![郵便配達人](assets/test-service-postman.JPG)
