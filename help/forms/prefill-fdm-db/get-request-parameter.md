---
title: リクエストパラメーターの取得
description: フォームデータモデルの事前入力サービスからリクエストパラメーターにアクセスする
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 49
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '176'
ht-degree: 100%

---

# リクエストパラメーターの取得

## empID パラメーターの取得

次の手順では、URL から empID パラメーターにアクセスします。empID リクエストパラメーターの値は、フォームデータモデルの **_GET_** サービス操作に渡されます。
このコースの目的に合わせて、以下を作成して提供しています。

* **_FDMDemo_** という名前のアダプティブフォームテンプレート
* **_fdmdemo_** という名前のページコンポーネント
* カスタム jsp を含むページコンポーネント
* アダプティブフォームテンプレートとページコンポーネントの関連付け

これにより、カスタム jsp 内のコードは、このカスタムテンプレートに基づくアダプティブフォームがレンダリングされたときにのみ実行されます

* [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を使用して[パッケージを読み込みます](assets/template-page-component.zip)
* [fdmrequest.jsp を開きます](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* コメントされた行のコメントを解除します。
* 変更を保存します。

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

empID の値は、paraMap の empID というキーに関連付けられています。次に、このマップは slingRequest に渡されます

>[!NOTE]
>
>キー empID は、新しいエンティティがサービスを取得する際の連結値と一致する必要があります

## 次の手順

[フォームデータモデルに基づくアダプティブフォームの作成](./create-adaptive-form.md)
