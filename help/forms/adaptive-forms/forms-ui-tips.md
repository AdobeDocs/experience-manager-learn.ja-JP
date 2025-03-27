---
title: 役に立つ UI のヒントとテクニック
description: 便利なユーザーインターフェイスのヒントを示すドキュメント
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-9270
last-substantial-update: 2019-06-09T00:00:00Z
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '147'
ht-degree: 100%

---

# パスワードフィールドの表示を切り替え

一般的なユースケースとしては、フォーム入力者が、「パスワード」フィールドに入力されたテキストの表示／非表示を切り替えることができます。
このユースケースを実現するために、[Font Awesome ライブラリ](https://fontawesome.com/)の目のアイコンを使用しました。このデモ用に作成されたクライアントライブラリには、必要な CSS と eye.svg が含まれています。


## サンプルコード

アダプティブフォームには、**ssnField** と呼ばれる PasswordBox タイプのフィールドがあります。

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

次の CSS を使用してパスワードフィールド内に&#x200B;**目**&#x200B;のアイコンを配置します

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

## パスワードサンプルの切り替えをデプロイします

* [クライアントライブラリ](assets/simple-ui-tips.zip)をダウンロードします
* [サンプルフォーム](assets/simple-ui-tricks-form.zip)をダウンロードします
* [パッケージマネージャー UI](http://localhost:4502/crx/packmgr/index.jsp) を使用してクライアントライブラリを読み込みます
* [フォームとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を使用してサンプルフォームを読み込みます
* [フォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/simpleuitips/jcr:content?wcmmode=disabled)


