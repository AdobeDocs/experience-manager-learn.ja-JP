---
title: リソースタイプを使用したサーブレットの登録
description: AEM Forms CS でサーブレットをリソースタイプにマッピングする
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 8%

---

# はじめに

パスによるサーブレットのバインドは、リソースタイプによるバインドと比べると、いくつかのデメリットがあります。つまり、

* パスバインドサーブレットは、デフォルトの JCR リポジトリ ACL を使用してアクセス制御することはできません
* パスバインドサーブレットは、パスにのみ登録でき、リソースタイプには登録できません（つまり、サフィックス処理なし）
* パスバインドサーブレットがアクティブでない場合（例：バンドルが見つからない場合や開始されていない場合）、POSTを実行すると予期しない結果が生じる場合があります。 通常、次の場所にノードを作成する `/bin/xyz` その後、マッピングをバインドするサーブレット・パスをオーバーレイするのは、リポジトリを見ている開発者に対しては透明ではありません。

## サーブレットの作成

IntelliJ で aem-banking プロジェクトを起動します。
次のスクリーンショットに示すように、サーブレットフォルダーの下に GetFieldChoices というサーブレットを作成します。
![選択肢](assets/fetchchoices.png)

## サンプルサーブレット

次のサーブレットは、Sling リソースタイプにバインドされます。 _**azure/fetchchoices**_



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

* ローカルのAEM SDK にログインします。
* という名前のリソースを作成する `fetchchoices` （このノードは好きに名前を付けることができます） `cq:Page` コンテンツノードの下に表示されます。
* 変更内容を保存します。
* という名前のノードを作成します。 `jcr:content` のタイプ `cq:PageContent` 変更を保存します。
* 次のプロパティを `jcr:content` ノード

| プロパティ名 | プロパティの値 |
|--------------------|--------------------|
| jcr:title | ユーティリティサーブレット |
| sling:resourceType | `azure/fetchchoices` |


The `sling:resourceType` 値は、サーブレットで指定された resourceTypes=&quot;azure/fetchchoices&quot;に一致する必要があります。

これで、を使用してリソースをリクエストすることで、サーブレットを呼び出せるようになりました。 `sling:resourceType` = `azure/fetchchoices` がそのフルパスに配置され、Sling サーブレットに登録されているセレクターまたは拡張機能がある。

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

パス `/content/fetchchoices/jcr:content` は、リソースと拡張のパスです。 `.json` は、サーブレットで指定されているものです

## AEMプロジェクトを同期

1. お気に入りのエディターでAEMプロジェクトを開きます。 私はこれに intelliJ を使った。
1. `\aem-banking-application\ui.content\src\main\content\jcr_root\content` の下に `fetchchoices` というフォルダーを作成します。
1. 右クリック `fetchchoices` フォルダーと選択 `repo | Get Command` （このメニュー項目は、このチュートリアルの前の章で設定します）。

このノードは、AEMからローカルのAEMプロジェクトに同期する必要があります。

AEMプロジェクト構造は次のようになります
![resource-resolver](assets/mapping-servlet-resource.png)
aem-banking-application\ui.content\src\main\content\META-INF\vaultフォルダーの filter.xml を次のエントリで更新します。

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

これで、Cloud Manager を使用して、変更をAEMas a Cloud Serviceの環境にプッシュできます。

## 次の手順

[フォームポータルコンポーネントの有効化](./forms-portal-components.md)



