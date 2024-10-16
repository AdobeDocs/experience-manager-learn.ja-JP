---
title: 第 7 章 - モバイルアプリからの AEM Content Services の使用 - Content Services
description: チュートリアルの第 7 章では、Android モバイルアプリを実行して、AEM Content Services から作成したコンテンツを使用します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 467
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '1350'
ht-degree: 100%

---

# 第 7 章 - モバイルアプリからの AEM Content Services の使用

チュートリアルの第 7 章では、ネイティブの Android モバイルアプリを使用して、AEM Content Services のコンテンツを使用します。

## Android モバイルアプリ

このチュートリアルでは、**シンプルなネイティブ Android モバイルアプリ**&#x200B;を使用して、AEM Content Services によって公開されたイベントコンテンツを使用および表示します。

[Android](https://developer.android.com/) の使用はほとんど重要ではありません。消費量の多いモバイルアプリは、iOS など、あらゆるモバイルプラットフォーム向けのあらゆるフレームワークで記述できます。

Android がチュートリアルに使われているのは、Windows、macOS、Linux で Android エミュレーターを実行できる機能や、その普及度、AEM 開発者がよく理解できる言語である Java として記述できることが理由です。

*チュートリアルの Android モバイルアプリは、Android Mobile アプリの作成方法を説明したり、Android 開発のベストプラクティスを伝えたりする方法を示すためのもの&#x200B;**ではなく**、AEM Content Services をモバイルアプリケーションから使用する方法を説明するためのものです。*

### AEM Content Services がモバイルアプリのエクスペリエンスを促進する仕組み

![モバイルアプリとコンテンツサービスとの対応付け](assets/chapter-7/content-services-mapping.png)

1. **ロゴ**。[!DNL Events API] ページの&#x200B;**画像コンポーネント**&#x200B;で定義されます。
1. **タグライン**。[!DNL Events API] ページの&#x200B;**テキストコンポーネント**&#x200B;で定義されます。
1. この&#x200B;**イベントリスト**&#x200B;は、イベントコンテンツフラグメントのシリアル化から派生したもので、設定済みの&#x200B;**コンテンツフラグメントリストコンポーネント**&#x200B;経由で公開されます。

## モバイルアプリのデモ

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### ローカルホスト以外で使用するためのモバイルアプリの設定

AEM パブリッシュが **http://localhost:4503** で実行されていない場合、ホストとポートはモバイルアプリの「[!DNL Settings]」で更新することが可能で、AEM パブリッシュのホスト／ポートのプロパティを指すようにできます。

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## モバイルアプリのローカルでの実行

1. [Android Studio](https://developer.android.com/studio/install) をダウンロードしてインストールし、Android Emulator をインストールします。
1. Android [!DNL APK] ファイルを [GitHub／Assets／wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) から&#x200B;**ダウンロード**&#x200B;します。
1. **Android Studio** を開きます。
   * Android Studio の初回起動時に、[!DNL Android SDK] のインストールを促すメッセージが表示されます。デフォルトを受け入れ、インストールを終了します。
1. Android Studio を開き、**「Profile」または「Debug APK」**&#x200B;を選択します。
1. 手順 2 でダウンロードした APK ファイル（**wknd-mobile.x.x.x.apk**）を選択し、「**OK**」をクリックします。
   * ダイアログで「**Create a New Folder**」か「**Use Existing**」を選択するように求められたら、「**Use Existing**」を選択します。
1. Android Studio の初回起動時に、「Projects」のリストで **wknd-mobile.x.x.x** を右クリックして、「**Open Module Settings**」を選択します。
   * **Modules／wknd-mobile.x.x.x／Dependencies タブ**&#x200B;の下で、「**Android API 29 Platform**」を選択します。「OK」をタップして閉じ、変更を保存します。
   * これを行わないと、エミュレーターを起動しようとするときに、「Android SDK を選択してください」という内容のエラーが表示されます。
1. **Tools／AVD Manager** と選択するか、上部バーの **AVD Manager** アイコンをタップして、**AVD Manager** を開きます。
1. **AVD Manager** ウィンドウで、「**+ Create Virtual Device...**」をクリックします（デバイスがまだ登録されていない場合）。
   1. 左側では、「**Phone**」カテゴリを選択します。
   1. 「**Pixel 2**」を選択します。
   1. 「**Next**」ボタンをクリックします。
   1. 「**Q**」を選択し、「**API Level 29**」を選択します。
      * AVD Manager の初回起動時に、バージョン管理された API をダウンロードするよう求められます。「Q」リリースの横にある「Download」リンクをクリックし、ダウンロードとインストールを完了します。
   1. 「**Next**」ボタンをクリックします。
   1. 「**Finish**」ボタンをクリックします。
1. **AVD Manager** ウィンドウを閉じます。
1. 上部メニューバーで、「**Run/Edit Configurations**」ドロップダウンから「**wknd-mobile.x.x.x**」を選択します。
1. 選択した「**Run/Edit Configuration**」の横にある「**Run**」ボタンをタップします。
1. ポップアップで、新しく作成した **[!DNL Pixel 2 API 29]** 仮想デバイスを選択し、「**OK**」をタップします。
1. [!DNL WKND Mobile] アプリがすぐに読み込まれない場合は、エミュレーターの Android ホーム画面から **[!DNL WKND]** アイコンを見つけてタップします。
   * エミュレーターが起動してもエミュレーターの画面が黒いままの場合は、エミュレーターウィンドウの横にあるエミュレーターのツールウィンドウの&#x200B;**電源**&#x200B;ボタンをタップします。
   * 仮想デバイス内をスクロールするには、クリックしたままドラッグします。
   * AEM からコンテンツを更新するには、更新アイコンが表示されるまで上部からプルダウンし、リリースします。

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## Mobile アプリコード

この節では、AEM コンテンツサービスとその JSON 出力に最も関連し、依存している Android Mobile アプリコードに焦点を当てます。

読み込み時に、Mobile アプリは `/content/wknd-mobile/en/api/events.model.json` に対して `HTTP GET` を行います。これは、Mobile アプリを駆動するコンテンツを提供するように設定された AEM コンテンツサービスエンドポイントです。

イベント API の編集可能なテンプレート（`/content/wknd-mobile/en/api/events.model.json`）がロックされているので、JSON 応答の特定の場所で特定の情報を検索するように Mobile アプリをコーディングできます。

### コードフローの概要

1. [!DNL WKND Mobile] アプリを開くと、`/content/wknd-mobile/en/api/events.model.json` にある AEM パブリッシュへの `HTTP GET` リクエストが呼び出され、コンテンツが収集されて Mobile アプリの UI に入力されます。
2. AEM からコンテンツを受信すると、Mobile アプリの 3 つのビュー要素である&#x200B;**ロゴ、タグライン、イベントリスト**&#x200B;のそれぞれが、AEM からのコンテンツで初期化されます。
   * AEM コンテンツを Mobile アプリのビュー要素にバインドするために、各 AEM コンポーネントを表す JSON は、Java POJO にマップされたオブジェクトであり、次に Android ビュー要素にバインドされます。
      * 画像コンポーネント JSON／ロゴ POJO／ロゴ ImageView
      * テキストコンポーネント JSON／TagLine POJO／テキスト ImageView
      * コンテンツフラグメントリスト JSON／イベント POJO／イベント RecyclerView
   * *モバイルアプリコードは、より大きな JSON 応答内の良く知られた場所により、JSON を POJO にマッピングできます。「image」、「text」、「contentfragmentlist」の JSON キーは、バッキング AEM コンポーネントのノード名によって決まることに注意してください。 これらのノード名が変更されると、JSON データから必要なコンテンツをソースする方法がわからないので、モバイルアプリは機能しなくなります。*

#### AEM コンテンツサービスエンドポイントの呼び出し

以下は、AEM Content Services を呼び出してモバイルアプリエクスペリエンスを駆動するコンテンツを収集するモバイル アプリの `MainActivity` 内のコードを要約したものです。

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` はモバイルアプリの初期化フックで、JSON の解析と値の `View` 要素へのバインドを担当する 3 つのカスタム `ViewBinders` を登録します。

次に `initApp(...)` が呼び出され、AEM パブリッシュ上の AEM Content Services エンドポイントに対して、コンテンツを収集するための HTTP GET リクエストが行われます。 有効な JSON 応答を受け取ると、JSON 応答が各 `ViewBinder` に渡されます。JSON が解析され、モバイル `View` 要素にバインドされます。

#### JSON 応答の解析

次の `LogoViewBinder` は簡単ですが、いくつかの重要な考慮事項に焦点を当てています。

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

`bind(...)` の最初の行は、コンポーネントが追加された AEM Layout Container を表すキー **:items → root → :items** を介して JSON 応答の下に移動します。

ここから、Image コンポーネントを表す **image** という名前のキーが確認されます（ここでも、このノード名 → JSON キーが安定していることが重要です）。このオブジェクトが存在する場合は、Jackson `ObjectMapper` ライブラリを介して読み取り、[custom Image POJO](#image-pojo) にマップします。Image POJO については、以下で説明します。

最後に、ヘルパーライブラリを使用して、ロゴの `src` が [!DNL Glide] Android ImageView に読み込まれます。

AEM スキーマ、ホスト、ポートを（`aemHost` 経由で）AEM パブリッシュインスタンスに提供する必要があることに注意してください。AEM コンテンツサービスは、参照されるコンテンツへの JCR パス（つまり `/content/dam/wknd-mobile/images/wknd-logo.png` ）のみを提供するためです。

#### Image POJO{#image-pojo}

（オプション） [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) また、Gson などの他のライブラリで提供される同様の機能は、複雑な JSON 構造を Java POJO にマッピングするのに役立ちます。ネイティブの JSON オブジェクト自体を直接操作するという退屈な作業は必要ありません。 この単純なケースでは、`image` JSON オブジェクトの `src` キーを、`@JSONProperty` 注釈を介して Image POJO の `src` 属性に直接マッピングします。

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

この手法は、`src` だけを必要とする単純な画像よりも、JSON オブジェクトからさらに多くのデータ ポイントを選択する必要があるイベント POJO に対してより大きなメリットがあります。

## モバイルアプリのエクスペリエンスを探索する

これで、AEM Content Services がネイティブのモバイルエクスペリエンスを促進する方法を理解できたので、学んだことを使用して次の手順を実行し、モバイルアプリに反映された変更を確認します。

各手順の後、モバイルアプリをプルして更新し、モバイルエクスペリエンスの更新を確認します。

1. **新しい[!DNL Event]コンテンツフラグメント**&#x200B;を作成して公開する
1. **既存の[!DNL Event]コンテンツフラグメント**&#x200B;を非公開にする
1. **タグライン**&#x200B;に更新を公開する

## これで完了です

**AEM ヘッドレスチュートリアルを完了しました。**

AEM Content Services と AEM as a Headless CMS の詳細については、Adobe のその他のドキュメントおよびイネーブルメント資料を参照してください。

* [コンテンツフラグメントの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html?lang=ja)
* [AEM WCM コアコンポーネントユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [AEM WCM コアコンポーネントコンポーネントライブラリ](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM コアコンポーネント GitHub プロジェクト](https://github.com/adobe/aem-core-wcm-components)
* [コンポーネントエクスポーターのコードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
