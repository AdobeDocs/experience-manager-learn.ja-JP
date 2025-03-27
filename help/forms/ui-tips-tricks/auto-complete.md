---
title: AEM Forms のオートコンプリート機能
description: 検索とフィルタリングを活用して、ユーザーが入力時に事前設定された値のリストから素早く検索および選択できるようにします。
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-11374
last-substantial-update: 2022-11-01T00:00:00Z
exl-id: e9a696f9-ba63-462d-93a8-e9a7a1e94e72
duration: 47
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '177'
ht-degree: 100%

---

# オートコンプリートの実装

jquery のオートコンプリート機能を使用して、AEM Forms にオートコンプリート機能を実装します。
この記事に含まれるサンプルでは、様々なデータソース（REST API 応答から生成された静的配列や動的配列）を使用して、ユーザーがテキストフィールドに入力を開始したときに候補を表示します。

オートコンプリート機能を実行するために使用されるコードは、フィールドの初期化イベントに関連付けられています。

## 住所の提案

![country-suggestions](assets/auto-complete2.png)



次に、住所の提案に使用するコードを示します

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```





## 絵文字を使用した提案

![country-suggestions](assets/auto-complete3.png)

候補リストに絵文字を表示するには、次のコードを使用します

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

[サンプルフォームは、こちらからダウンロードできます](assets/auto-complete-form.zip)。REST 呼び出しを正常に行うには、コードのコードエディターを使用して独自のユーザー名／API キーを指定します。

>[!NOTE]
>
> オートコンプリートを機能させるには、フォームで次のクライアントライブラリ **cq.jquery.ui** が使用されていることを確認します。このクライアントライブラリは AEM に付属しています。
