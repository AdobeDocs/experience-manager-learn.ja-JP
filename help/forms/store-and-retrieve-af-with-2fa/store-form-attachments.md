---
title: フォーム添付ファイルの保存
description: フォームの添付ファイルを抽出して、CRX リポジトリ内の新しい場所に保存します。
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
duration: 65
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '192'
ht-degree: 100%

---

# フォーム添付ファイルの保存

アダプティブフォームに添付ファイルを追加すると、添付ファイルは CRX リポジトリ内の一時的な場所に保存されます。 このユースケースが機能するには、CRX リポジトリ内の新しい場所にフォームの添付ファイルを保存する必要があります。

OSGi サービスを作成して、CRX リポジトリ内の新しい場所にフォームの添付ファイルを保存します。 CRX 内の添付ファイルの新しい場所を使用して新しいファイルマップが作成され、呼び出し元のアプリケーションに返されます。
サーブレットに送信される FileMap を以下に示します。 キーはアダプティブフォームフィールドで、値は添付ファイルの一時的な場所です。 このサーブレットでは、添付ファイルを抽出して AEM リポジトリ内の新しい場所に保存し、その新しい場所を FileMap に反映させます。

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

リクエストから添付ファイルを抽出して **/content/afattachments** フォルダーに保存するコードを以下に示します。

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

これは、フォームの添付ファイルの場所が更新された新しい FileMap です。

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## 次の手順

[フォームデータの保存](./store-form-data.md)