---
title: クエリパラメーターを使用してアダプティブFormsを設定します。
description: クエリパラメーターからのデータをアダプティブFormsに入力します。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
source-git-commit: fad7630d2d91d03b98a3982f73a689ef48700319
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# クエリパラメーターを使用したアダプティブFormsの事前入力

1 人のお客様は、クエリパラメーターを使用してアダプティブフォームに入力する必要がありました。 例えば、次の URL では、アダプティブフォームの「FirstName」フィールドと「LastName」フィールドはそれぞれ「John」と「Doe」に設定されています

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

この使用例を実現するために、新しいアダプティブフォームテンプレートが作成され、ページコンポーネントに関連付けられました。 このページコンポーネントには、クエリーパラメーターを取得し、アダプティブフォームの入力に使用できる xml 構造を作成するための jsp があります。

新しいアダプティブフォームテンプレートとページコンポーネントの作成について詳しくは、次のとおりです。 [このビデオで説明します。](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

次に、jsp ページで使用されたコードを示します

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>フォームでスキーマを使用している場合、xml の構造は異なり、それに応じて xml を構築する必要があります。


## システムにアセットをデプロイする

* [パッケージマネージャーを使用してアダプティブフォームテンプレートをダウンロードし、インストールする](assets/populate-with-xml.zip)
* [サンプルのアダプティブフォームをダウンロードしてインストールする](assets/populate-af-with-query-paramters-form.zip)

* [アダプティブフォームのプレビュー](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
アダプティブフォームが John と Doe の値で入力されているのが確認できます。
