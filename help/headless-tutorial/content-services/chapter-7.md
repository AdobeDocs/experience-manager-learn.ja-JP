---
title: 第 7 章 — モバイルアプリからのAEM Content Services の使用 — Content Services
description: チュートリアルの第 7 章では、AEM Content Services から作成したコンテンツを使用するために Android モバイルアプリを実行します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1391'
ht-degree: 1%

---

# 第 7 章 — モバイルアプリからのAEM Content Services の使用

チュートリアルの第 7 章では、ネイティブの Android モバイルアプリを使用して、AEM Content Services のコンテンツを使用します。

## Android モバイルアプリ

このチュートリアルでは、 **シンプルなネイティブ Android モバイルアプリ** を使用して、AEM Content Services によって公開されたイベントコンテンツを使用および表示します。

の使用 [Android](https://developer.android.com/) はほとんど重要ではありません。消費量の多いモバイルアプリは、iOSなど、あらゆるモバイルプラットフォーム向けのあらゆるフレームワークで記述できます。

Android は、Windows、macOS、Linux で Android エミュレーターを実行する機能とその人気、AEM開発者がよく理解できる言語である Java として記述できることから、チュートリアルに使用されます。

*チュートリアルの Android モバイルアプリは、**not**は、Android Mobile アプリの作成方法を説明したり、Android 開発のベストプラクティスを伝えたりする方法を示したもので、AEM Content Services をモバイルアプリケーションで使用する方法を説明したものです。*

### AEM Content Services がモバイルアプリのエクスペリエンスを促進する仕組み

![モバイルアプリとコンテンツサービスとの対応付け](assets/chapter-7/content-services-mapping.png)

1. この **ロゴ** で定義されたとおり [!DNL Events API] ページの **画像コンポーネント**.
1. この **タグ行** 次の項で定義されたとおり： [!DNL Events API] ページの **テキストコンポーネント**.
1. この **イベントリスト** は、設定済みの **コンテンツフラグメントリストコンポーネント**.

## モバイルアプリのデモ

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### ローカルホスト以外で使用するためのモバイルアプリの設定

AEM パブリッシュがで実行されていない場合 **http://localhost:4503** ホストとポートは、モバイルアプリの [!DNL Settings] をクリックして、AEM パブリッシュのホスト/ポートプロパティを指します。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## モバイルアプリのローカルでの実行

1. をダウンロードしてインストールする [Android Studio](https://developer.android.com/studio/install) :Android エミュレーターをインストールします。
1. **ダウンロード** Android [!DNL APK] ファイル [GitHub / Assets / wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. 開く **Android Studio**
   * Android Studio の初回起動時に、 [!DNL Android SDK] が存在する場合。 デフォルトを受け入れ、インストールを終了します。
1. Android Studio を開き、「 」を選択します。 **プロファイルまたは Debug APK**
1. APK ファイル (**wknd-mobile.x.x.x.apk**) を手順 2 でダウンロードし、「 」をクリックします。 **OK**
   * 次のように求められた場合： **新しいフォルダーを作成**&#x200B;または **既存を使用**&#x200B;を選択します。 **既存を使用**.
1. Android Studio の初回起動時に、 **wknd-mobile.x.x.x** 「プロジェクト」リストで、「 **モジュール設定を開く**.
   * の下 **モジュール/ wknd-mobile.x.x.x /「依存関係」タブ**&#x200B;を選択します。 **Android API 29 プラットフォーム**. 「 OK 」をタップして、変更を閉じて保存します。
   * これをおこなわないと、エミュレーターを起動しようとすると、「Android SDK を選択してください」というエラーが表示されます。
1. を開きます。 **AVD マネージャ** 選択する **ツール/AVD マネージャ** または **AVD マネージャ** アイコンをクリックします。
1. 内 **AVD マネージャ** ウィンドウ、クリック **+仮想デバイスを作成…** （デバイスがまだ登録されていない場合）。
   1. 左側で、 **電話** カテゴリ。
   1. を選択します。 **ピクセル 2**.
   1. 次をクリック： **次へ** 」ボタンをクリックします。
   1. 選択 **Q** と **API レベル 29**.
      * AVD Manager の初回起動時に、「バージョン管理された API をダウンロードする」よう求められます。 「Q」リリースの横にある「ダウンロード」リンクをクリックし、ダウンロードとインストールを完了します。
   1. 次をクリック： **次へ** 」ボタンをクリックします。
   1. 次をクリック： **完了** 」ボタンをクリックします。
1. を閉じる **AVD マネージャ** ウィンドウ
1. 上部のメニューバーで、を選択します。 **wknd-mobile.x.x.x** から **実行/編集設定** ドロップダウンします。
1. 次をタップします。 **実行** 選択した **設定を実行/編集**
1. ポップアップで、新しく作成した **[!DNL Pixel 2 API 29]** 仮想デバイスとタップ **OK**
1. この [!DNL WKND Mobile] アプリがすぐに読み込まれず、 **[!DNL WKND]** アイコンをクリックします。
   * エミュレーターが起動したがエミュレーターの画面が黒のままの場合は、 **電源** ボタンが表示されます。
   * 仮想デバイス内をスクロールするには、クリック&amp;ホールドしてドラッグします。
   * AEMからコンテンツを更新するには、更新アイコンが表示されるまで上部からプルダウンし、リリースします。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## モバイルアプリコード

この節では、最もやり取りし、AEM Content Services とその JSON 出力に依存する Android モバイルアプリコードについて説明します。

読み込み時に、モバイルアプリが `HTTP GET` から `/content/wknd-mobile/en/api/events.model.json` ( モバイルアプリを駆動するコンテンツを提供するように設定されたAEM Content Services エンドポイント )

イベント API の編集可能なテンプレート (`/content/wknd-mobile/en/api/events.model.json`) がロックされている場合、モバイルアプリをコードして、JSON 応答内の特定の場所にある特定の情報を探すことができます。

### 高レベルのコードフロー

1. を開く [!DNL WKND Mobile] アプリが `HTTP GET` 次の場所で AEM パブリッシュにリクエスト： `/content/wknd-mobile/en/api/events.model.json` をクリックして、モバイルアプリの UI に入力するコンテンツを収集します。
2. AEMからコンテンツを受け取ると、モバイルアプリの 3 つのビュー要素のそれぞれが、 **ロゴ、タグライン、イベントリスト**&#x200B;がAEMからのコンテンツで初期化されます。
   * AEMコンテンツをモバイルアプリのビュー要素にバインドする場合、各AEMコンポーネントを表す JSON は、Java POJO にオブジェクトがマッピングされ、その後、Android ビュー要素にバインドされます。
      * 画像コンポーネント JSON →ロゴ POJO →ロゴ ImageView
      * テキストコンポーネント JSON → TagLine POJO →テキスト ImageView
      * コンテンツフラグメントリスト JSON →イベント POJO →イベントリサイクラービュー
   * *モバイルアプリコードは、より大きな JSON 応答内の既知の場所が原因で、JSON を POJO にマッピングできます。 「image」、「text」、「contentfragmentlist」の JSON キーは、バッキングAEMコンポーネントのノード名によって決まることに注意してください。 これらのノード名が変更されると、JSON データから必要なコンテンツをソースする方法がわからないので、モバイルアプリは機能しなくなります。*

#### AEM Content Services エンドポイントの呼び出し

以下は、モバイルアプリの `MainActivity` は、AEM Content Services を呼び出して、モバイルアプリエクスペリエンスを駆動するコンテンツを収集します。

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

`onCreate(..)` はモバイルアプリの初期化フックで、3 つのカスタム `ViewBinders` JSON を解析し、値を `View` 要素。

`initApp(...)` が呼び出され、AEM パブリッシュ上のAEM Content Services エンドポイントに対して、コンテンツを収集するための HTTPGETリクエストがおこなわれます。 有効な JSON 応答を受け取ると、JSON 応答が各 `ViewBinder` JSON を解析し、モバイルにバインドする役割を持つ `View` 要素。

#### JSON 応答の解析

次に、 `LogoViewBinder`は簡単ですが、いくつかの重要な考慮事項に焦点を当てています。

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

最初の行 `bind(...)` キーを使用して JSON 応答を下に移動します。 **:items → root → :items** これは、コンポーネントが追加されたAEMレイアウトコンテナを表します。

ここから、という名前のキーのチェックが行われます。 **画像**：画像コンポーネントを表します ( これも、このノード名→ JSON キーが安定していることが重要です )。 このオブジェクトが存在する場合は、読み取られ、 [カスタム画像 POJO](#image-pojo) ジャクソンを通して `ObjectMapper` ライブラリ。 画像の POJO については、以下で説明します。

最後に、ロゴの `src` は、 [!DNL Glide] ヘルパーライブラリ。

AEMスキーマ、ホスト、ポート ( `aemHost`) をAEM Content Services として AEM パブリッシュインスタンスに渡す場合、JCR パス ( `/content/dam/wknd-mobile/images/wknd-logo.png`) を参照元のコンテンツに追加します。

#### 画像 POJO{#image-pojo}

（オプション） [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) また、Gson などの他のライブラリで提供される同様の機能は、複雑な JSON 構造を Java POJO にマッピングするのに役立ちます。ネイティブの JSON オブジェクト自体を直接操作するというティジウムは必要ありません。 この単純なケースでは、 `src` キー `image` JSON オブジェクト ( `src` 属性を `@JSONProperty` 注釈。

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

JSON オブジェクトから多くのデータポイントを選択する必要があるイベント POJO は、単純な画像よりもこの方法のメリットが大きいです。この方法は、次のようなものです。 `src`.

## モバイルアプリのエクスペリエンスを見る

これで、AEM Content Services がネイティブの Mobile エクスペリエンスを促進する方法を理解できたので、以下の手順を実行して、変更をモバイルアプリに反映させる方法を学習しました。

各手順の後、モバイルアプリをプルして更新し、モバイルエクスペリエンスの更新を確認します。

1. 作成して公開 **新規 [!DNL Event] コンテンツフラグメント**
1. 非公開 **既存 [!DNL Event] コンテンツフラグメント**
1. に更新を公開 **タグライン**

## これで完了です

**AEMヘッドレスチュートリアルを完了しました。**

AEMコンテンツサービスとAEM as a Headless CMS の詳細については、Adobeのその他のドキュメントおよび実施可能資料を参照してください。

* [コンテンツフラグメントの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCM コアコンポーネントユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)
* [AEM WCM コアコンポーネントコンポーネントライブラリ](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCM コアコンポーネント GitHub プロジェクト](https://github.com/adobe/aem-core-wcm-components)
* [コンポーネントエクスポーターのコードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
