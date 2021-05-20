---
title: リクエストパラメーターの取得
description: フォームデータモデルの事前入力サービスから要求パラメーターにアクセスする
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 4%

---

# リクエストパラメーターの取得

## empIDパラメータの取得

次の手順では、URLからempIDパラメータにアクセスします。 次に、empID要求パラメーターの値が、フォームデータモデルの&#x200B;**_get_**サービス操作に渡されます。
このコースの目的に合わせて、以下を作成し、提供しました

* **_FDMDemo_**&#x200B;というアダプティブフォームテンプレート
* **_fdmemo_**&#x200B;という名前のページコンポーネント
* ページコンポーネントにカスタムjspを含めました。
* アダプティブフォームテンプレートとページコンポーネントを関連付ける

これにより、カスタムjsp内のコードは、このカスタムテンプレートに基づくアダプティブフォームがレンダリングされた場合にのみ実行されます

* [パッケージマネージャー](assets/template-page-component.zip) を使用したパ [ッケージの読み込み](http://localhost:4502/crx/packmgr/index.jsp)
* [fdmrequest.jspを開きます。](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* コメント行のコメントを解除します。
* 変更を保存します

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empIDの値は、paraMapでempIDと呼ばれるキーに関連付けられます。 次に、このマップがslingRequestに渡されます

>[!NOTE]
>
>キーempIDは、newhireエンティティがサービスを受け取るバインディング値と一致する必要があります
