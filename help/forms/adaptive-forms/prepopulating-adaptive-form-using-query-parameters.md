---
title: クエリパラメーターを使用してアダプティブフォームを設定します。
description: クエリパラメーターからのデータでアダプティブフォームを設定します。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
duration: 49
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '199'
ht-degree: 100%

---

# クエリパラメーターを使用したアダプティブフォームの事前設定

お客様の 1 人は、クエリパラメーターを使用してアダプティブフォームに入力する必要がありました。例えば、次の URL では、アダプティブフォームの FirstName フィールドと LastName フィールドがそれぞれ John と Doe に設定されています。

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

このユースケースを実現するために、新しいアダプティブフォームテンプレートが作成され、ページコンポーネントに関連付けられました。このページコンポーネントには、クエリパラメーターを取得し、アダプティブフォームへの入力に使用できる xml 構造を作成するための jsp があります。

新しいアダプティブフォームテンプレートとページコンポーネントの作成について詳しくは、[このビデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=ja)をご覧ください。

次に、jsp ページで使用されたコードを示します。

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
>フォームでスキーマを使用している場合、xml の構造は異なり、それに応じて xml を作成する必要があります。


## システムへのアセットのデプロイ

* [パッケージマネージャーを使用してアダプティブフォームテンプレートをダウンロードし、インストールする](assets/populate-with-xml.zip)
* [サンプルのアダプティブフォームをダウンロードしてインストールする](assets/populate-af-with-query-paramters-form.zip)

* [アダプティブフォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
アダプティブフォームに John と Doe の値が入力されているのが確認できます。
