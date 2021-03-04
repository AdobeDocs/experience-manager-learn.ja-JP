---
title: リクエストパラメーターの取得
description: フォームデータモデルの事前入力サービスからリクエストパラメーターにアクセスする
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: 開発
role: デベロッパー
level: 初心者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 4%

---

# リクエストパラメーターの取得

## empIDパラメータの取得

次の手順では、URLからempIDパラメータにアクセスします。 次に、empIDリクエストパラメーターの値が、フォームデータモデルの&#x200B;**_get_**サービス操作に渡されます。
このコースの目的に合わせて、以下を作成し、提供します。

* **_FDMDemo_**&#x200B;という名前のアダプティブフォームテンプレート
* **_fdmemo_**&#x200B;というページコンポーネント
* ページコンポーネントにカスタムJSPを含めました
* アダプティブフォームテンプレートとページコンポーネントとの関連付け

これを行うと、カスタムjspのコードは、このカスタムテンプレートに基づくアダプティブフォームがレンダリングされたときにのみ実行されます

* [パッケー](assets/template-page-component.zip) ジマネージャーを使用した [パッケージの読み込み](http://localhost:4502/crx/packmgr/index.jsp)
* [fdmrequest.jspを開きます。](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* コメント化された行のコメントを解除します。
* 変更を保存する

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empIDの値は、paraMapのempIDというキーに関連付けられます。 その後、このマップがslingRequestに渡されます

>[!NOTE]
>
>empIDキーは、newhireエンティティのget serviceの連結値と一致する必要があります
