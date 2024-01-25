---
title: ユーザー指定のメタデータタグを追加するためのチュートリアル
description: カスタム送信を作成して、Azure にメタデータタグ付きのフォームデータを保存する
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 54
exl-id: eb9bcd1b-c86f-4894-bcf8-9c17e74dd6ec
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 1%

---

# BLOB インデックスタグを追加

データセットが大きくなると、データの海で特定のオブジェクトを見つけるのが難しくなる場合があります。 BLOB インデックスタグは、キー値インデックスタグ属性を使用して、データの管理と検出の機能を提供します。 単一のコンテナ内、またはストレージアカウント内のすべてのコンテナ間で、オブジェクトを分類して検索できます。 例： blob インデックスタグ _**CustomerType=Platinum**_（ここで、プラチナはフィールド CustomerType の値です）。

![index-tags](assets/blob-with-index-tags1.png)
次のコードは、送信されたデータから対応する値を持つ blob インデックスデータタグ文字列を作成します

```java
@Override
    public String getMetaDataTags(String submittedFormName,String formPath,Session session,String formData) {

        JsonObject jsonObject = JsonParser.parseString(formData).getAsJsonObject();
        List<String>metaDataTags = new ArrayList<String>();
        metaDataTags.add("formName="+submittedFormName);
        Map< String, String > map = new HashMap< String, String >();
        map.put("path", formPath);
        map.put("1_property", "Searchable");
        map.put("1_property.value","true");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map),session);
        query.setStart(0);
        query.setHitsPerPage(20);
        SearchResult result = query.getResult();
        logger.debug("Get result hits " + result.getHits().size());
        for (Hit hit: result.getHits()) {
            try {
                    logger.debug(hit.getPath());
                    String jsonElementName = (String) hit.getProperties().get("name");
                    String fieldName = hit.getProperties().get("name").toString();
                    if(jsonObject.get(jsonElementName).isJsonArray())
                    {
                        
                        JsonArray arrayOfValues = jsonObject.get(jsonElementName).getAsJsonArray();
                        StringBuilder valuesString = new StringBuilder();
                        for(int j=0;j<arrayOfValues.size();j++)
                        {
                            valuesString.append(arrayOfValues.get(j).getAsString());
                            if(j < arrayOfValues.size() -1)
                            {
                                valuesString.append(" and ");
                            }
                        }

                        
                        metaDataTags.add(fieldName + "=" + valuesString.toString());

                    }
                    else
                    {
                        logger.debug("The searchable field name is " + fieldName + "the json element name is " + jsonElementName);
                        metaDataTags.add(fieldName + "=" + jsonObject.get(jsonElementName).getAsString());
                    }

            } catch (RepositoryException e) {
                throw new RuntimeException(e);
            }
        }
        return String.join("&",metaDataTags);
    }
```

## 次の手順

[カスタム送信ハンドラーを作成](./create-custom-submit.md)
