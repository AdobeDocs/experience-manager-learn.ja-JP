---
title: リソースタイプを使用したサーブレットの登録
description: AEM Forms CS でサーブレットをリソースタイプにマッピングする
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 90
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 100%

---

# はじめに

パスによるサーブレットのバインドは、リソースタイプによるバインドと比べると、いくつかのデメリットがあります。

* パスバインドサーブレットでは、デフォルトの JCR リポジトリ ACL を使用してアクセス制御することはできません
* パスバインドサーブレットは、パスにのみ登録でき、リソースタイプには登録できません（例えば、サフィックス処理はありません）
* パスバインドサーブレットがアクティブでない場合（例：バンドルが見つからない場合や開始されていない場合）、POST を実行すると予期しない結果が生じる場合があります。通常、`/bin/xyz` にノードを作成すると、その後にマッピングをバインドするサーブレットパスをオーバーレイします。
開発者がリポジトリを見るだけでは透過的ではありません。
このような欠点を考慮すると、サーブレットをパスではなく、リソースタイプにバインドすることを強くお勧めします。

## サーブレットの作成

IntelliJ で aem-banking プロジェクトを起動します。次のスクリーンショットに示すように、サーブレットフォルダーの下に GetFieldChoices というサーブレットを作成します。
![選択肢](assets/fetchchoices.png)

## サンプルサーブレット

次のサーブレットは、_**azure/fetchchoices**_ の Sling リソースタイプにバインドされます。



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## CRX でのリソースの作成

* ローカルの AEM SDK にログインします。
* コンテンツノードの下に `cq:Page` タイプの `fetchchoices` という名前のリソース（このノードには好きに名前を付けることができます）リソースを作成します。
* 変更内容を保存します。
* `cq:PageContent` のタイプの `jcr:content` という名前のノードを作成して変更を保存します。
* `jcr:content` ノードに次のプロパティを追加します。

| プロパティ名 | プロパティの値 |
|--------------------|--------------------|
| jcr:title | ユーティリティサーブレット |
| sling:resourceType | `azure/fetchchoices` |


`sling:resourceType` 値は、サーブレットで指定された「resourceTypes=&quot;azure/fetchchoice」に一致する必要があります。

これで、Sling サーブレットに登録されているセレクターまたは拡張機能を使用し、リソースのフルパスに `sling:resourceType` = `azure/fetchchoices` を指定してリクエストして、サーブレットを呼び出すことができるようになりました。

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

`/content/fetchchoices/jcr:content` のパスは、リソースと `.json` の拡張のパスで、サーブレットで指定されています。

## AEM プロジェクトを同期する

1. 使い慣れたエディターで AEM プロジェクトを開きます。この記事では intelliJ を使用しました。
1. `\aem-banking-application\ui.content\src\main\content\jcr_root\content` の下に `fetchchoices` というフォルダーを作成します。
1. `fetchchoices` フォルダーを右クリックして「`repo | Get Command`」を選択（このメニュー項目は、このチュートリアルの前の章で設定します）します。

このノードは、AEM からローカルの AEM プロジェクトに同期する必要があります。

AEM プロジェクト構造は次のようになります。
![resource-resolver](assets/mapping-servlet-resource.png)
aem-banking-application\ui.content\src\main\content\META-INF\vault フォルダーの filter.xml を次のエントリで更新します。

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

これで、Cloud Manager を使用して、変更をAEM as a Cloud Service 環境にプッシュできます。

## 次の手順

[フォームポータルコンポーネントの有効化](./forms-portal-components.md)
