---
title: フォームの添付ファイルの保存
description: フォームの添付ファイルを抽出し、CRXリポジトリの新しい場所に保存します。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6537
thumbnail: 6537.jpg
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 1%

---

# フォームの添付ファイルの保存

アダプティブフォームに添付ファイルを追加すると、添付ファイルはCRXリポジトリの一時的な場所に保存されます。 使用事例が正しく動作するためには、フォーム添付ファイルをCRXリポジトリの新しい場所に保存する必要があります。

OSGiサービスは、フォーム添付ファイルをCRXリポジトリの新しい場所に保存するために作成されます。 CRX内の添付ファイルの新しい場所を持つ新しいファイルマップが作成され、呼び出し元のアプリケーションに戻ります。
次に、サーブレットに送信されるFileMapを示します。 キーはアダプティブフォームのフィールドで、値は添付ファイルの一時的な場所です。 サーブレットでは、添付ファイルを抽出し、AEMリポジトリの新しい場所に保存し、新しい場所でFileMapを更新します

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

次のコードは、リクエストから添付ファイルを抽出し、**/content/afattachments**&#x200B;フォルダーに保存するコードです

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

これは、フォーム添付ファイルの更新された場所を持つ新しいFileMapです

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```
