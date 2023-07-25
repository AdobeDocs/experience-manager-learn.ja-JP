---
title: 新しいページコンポーネントを作成
description: 新しいページコンポーネントの作成
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 12%

---


# ページコンポーネント 

ページコンポーネントは、ページのレンダリングを担当する通常のコンポーネントです。 新しいページコンポーネントを作成し、このページコンポーネントを新しいアダプティブフォームテンプレートに関連付けます。 これにより、アダプティブフォームがこの特定のテンプレートに基づいている場合にのみ、アドビのコードが実行されるようになります。

## ページコンポーネントを作成

ローカルクラウド対応のAEM Formsインスタンスにログインします。 apps フォルダーの下に次の構造を作成します。
![page-component](./assets/page-component1.png)

1. ページフォルダーを右クリックし、タイプ cq:Component の storeandfetch というノードを作成します。
1. 変更内容を保存します
1. 次のプロパティを `storeandfetch` ノードと保存

| **プロパティ名** | **プロパティタイプ** | **プロパティの値** |
|-------------------------|-------------------|----------------------------------------|
| componentGroup | 文字列 | hidden |
| jcr :description | 文字列 | アダプティブフォームテンプレートのページタイプ |
| jcr:title | 文字列 | アダプティブフォームテンプレートのページ |
| sling:resourceSuperType | 文字列 | `fd/af/components/page2/aftemplatedpage` |

を `/libs/fd/af/components/page2/aftemplatedpage/aftemplatedpage.jsp` そしてそれを `storeandfetch` ノード。 名前を変更 `aftemplatedpage.jsp` から `storeandfetch.jsp`.

開く `storeandfetch.jsp` 次の行を追加します。

```jsp
<cq:include script="azureportal.jsp"/>
```

の下に

```jsp
<cq:include script="fallbackLibrary.jsp"/>
```

最終的なコードは次のようになります

```jsp
<cq:include script="fallbackLibrary.jsp"/>
<cq:include script="azureportal.jsp"/>
```

storeandfetch ノードの下に azureportal.jsp という名前のファイルを作成し、次のコードを azureportal.jsp にコピーして、変更を保存します。

```jsp
<%@page session="false" %>
<%@include file="/libs/fd/af/components/guidesglobal.jsp" %>
<%@ page import="org.apache.commons.logging.Log" %>
<%@ page import="org.apache.commons.logging.LogFactory" %>
<%
    if(request.getParameter("guid")!=null) {
            logger.debug( "Got Guid in the request" );
            String BlobId = request.getParameter("guid");
            java.util.Map paraMap = new java.util.HashMap();
            paraMap.put("BlobId",BlobId);
            slingRequest.setAttribute("paramMap",paraMap);
    } else {
            logger.debug( "There is no Guid in the request " );
    }            
%>
```

このコードでは、リクエストパラメーターの値を取得します **素晴らしい** BlobId という変数に格納します。 次に、この BlobId は、 paramMap 属性を使用して Sling 要求に渡されます。 このコードが機能するには、Azure Storage ベースのフォームデータモデルに基づくフォームが存在し、フォームデータモデルの読み取りサービスが、以下のスクリーンショットに示すように、BlobId と呼ばれる要求属性にバインドされていると想定します。

![fdm-request-attribute](./assets/fdm-request-attribute.png)

### 次の手順

[ページコンポーネントとテンプレートの関連付け](./associate-page-component.md)

