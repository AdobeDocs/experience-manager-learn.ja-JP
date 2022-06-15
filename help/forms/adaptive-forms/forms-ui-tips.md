---
title: 役に立つ UI のヒントとテクニックをいくつか紹介します。
description: 便利なユーザーインターフェイスのヒントを示すドキュメント
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 9270
source-git-commit: c462d48d26c9a7aa0e4cfc4f24005b41e8e82cb8
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 10%

---

# パスワードフィールドの表示を切り替え

一般的な使用例としては、フォーム入力者が、パスワードフィールドに入力されたテキストの表示/非表示を切り替えることができます。
この使用例を達成するために、 [Font Awesome ライブラリ](https://fontawesome.com/). このデモ用に作成されたクライアントライブラリには、必要な CSS と eye.svg が含まれています。


## サンプルコード

アダプティブフォームには、という名前のタイプの PasswordBox フィールドがあります。 **ssnField**.

フォームが読み込まれると、次のコードが実行されます

```javascript
$(document).ready(function() {
    $(".ssnField").append("<p id='fisheye' style='color: Dodgerblue';><i class='fa fa-eye'></i></p>");

    $(document).on('click', 'p', function() {
        var type = $(".ssnField").find("input").attr("type");

        if (type == "text") {
            $(".ssnField").find("input").attr("type", "password");
        }

        if (type == "password") {
            $(".ssnField").find("input").attr("type", "text");
        }

    });

});
```

次の CSS を使用して **目** パスワードフィールド内のアイコン

```javascript
.svg-inline--fa {
    /* display: inline-block; */
    font-size: inherit;
    height: 1em;
    overflow: visible;
    vertical-align: -.125em;
    position: absolute;
    top: 45px;
    right: 40px;
}
```

## 切り替えパスワードのサンプルをデプロイします。

* をダウンロードします。 [クライアントライブラリ](assets/simple-ui-tips.zip)
* をダウンロードします。 [サンプルフォーム](assets/simple-ui-tricks-form.zip)
* を使用してクライアントライブラリを読み込む [パッケージマネージャー UI](http://localhost:4502/crx/packmgr/index.jsp)
* 次を使用してサンプルフォームを読み込みます： [Formsとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


