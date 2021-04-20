---
title: 第7章 — モバイルアプリからのAEM Content Servicesの利用 — Content Services
description: チュートリアルの第7章では、AEM Content Servicesで作成したコンテンツをAndroidモバイルアプリで使用する方法を説明します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1414'
ht-degree: 1%

---


# 第7章 — モバイルアプリからのAEM Content Servicesの利用

チュートリアルの第7章では、ネイティブのAndroidモバイルアプリを使用して、AEM Content Servicesのコンテンツを参照します。

## Androidモバイルアプリ

このチュートリアルでは、**単純なネイティブAndroidモバイルアプリ**&#x200B;を使用して、AEM Content Servicesで公開されたイベントコンテンツを使用し、表示します。

[Android](https://developer.android.com/)の使用はほとんど重要ではありません。使用するモバイルアプリは、iOSなど、どのモバイルプラットフォーム用のフレームワークでも記述できます。

Androidは、Windows、macOS、LinuxでAndroidエミュレーターを実行する機能と、その人気、AEM開発者による理解が深いJavaとして記述できることから、チュートリアルに使用されます。

*チュートリアルのAndroidモバイルアプリは、Androidモバイルアプリの作成方法やAndroid開発のベストプラクティスを伝える方法を&#x200B;****意図したものではありませんが、AEM Content Servicesをモバイルアプリケーションで利用する方法を説明したものです。*

### AEM Content ServicesがMobile Appのエクスペリエンスを引き起こす仕組み

![モバイルアプリとContent Servicesのマッピング](assets/chapter-7/content-services-mapping.png)

1. [!DNL Events API]ページの&#x200B;**画像コンポーネント**&#x200B;で定義される&#x200B;**ロゴ**。
1. [!DNL Events API]ページの&#x200B;**テキストコンポーネント**&#x200B;で定義されている&#x200B;**タグ行**。
1. この&#x200B;**イベントリスト**&#x200B;は、設定された&#x200B;**コンテンツフラグメントリストコンポーネント**&#x200B;を介して公開されたイベントコンテンツフラグメントのシリアル化から派生します。

## モバイルアプリのデモ

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### ローカルホスト以外で使用するためのモバイルアプリの設定

AEM Publishが&#x200B;**http://localhost:4503**&#x200B;で実行されていない場合は、AEM Publishのホスト/ポートのプロパティを指すように、モバイルアプリの[!DNL Settings]でホストとポートを更新できます。

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## モバイルアプリをローカルで実行する

1. [Android Studio](https://developer.android.com/studio/install)をダウンロードしてインストールし、Androidエミュレーターをインストールします。
1. **Android** ファイル [!DNL APK]  [のダウンロードGitHub/Assets/wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. **Android Studio**&#x200B;を開きます
   * Android Studioの初回起動時に、[!DNL Android SDK]をインストールするように求めるプロンプトが表示されます。 デフォルトを受け入れ、インストールを終了します。
1. Android Studioを開き、**プロファイルまたはデバッグAPK**&#x200B;を選択します。
1. 手順2でダウンロードしたAPKファイル(**wknd-mobile.x.x.apk**)を選択し、「**OK**」をクリックします
   * 「**新しいフォルダーを作成**」または「**既存の**&#x200B;を使用」を選択するよう求められた場合は、「**既存の**&#x200B;を使用」を選択します。
1. Android Studioの初回起動時に、プロジェクトリストーの&#x200B;**wknd-mobile.x.x**&#x200B;を右クリックし、「**モジュール設定を開く**」を選択します。
   * **モジュール/wknd-mobile.x.x/「依存関係」タブ**&#x200B;の下で、**Android API 29プラットフォーム**&#x200B;を選択します。 「OK」をタップして、変更を閉じ、保存します。
   * この操作を行わないと、エミュレーターを起動しようとすると、「Android SDKを選択してください」というエラーが表示されます。
1. **AVDマネージャー**&#x200B;を開くには、**ツール/AVDマネージャー**&#x200B;を選択するか、上部のバーの&#x200B;**AVDマネージャー**&#x200B;アイコンをタップします。
1. **AVDマネージャ**&#x200B;ウィンドウで、**+仮想デバイスの作成…をクリックします。**&#x200B;を返します。
   1. 左側で、「**電話**」カテゴリを選択します。
   1. **ピクセル2**&#x200B;を選択します。
   1. 「**次へ**」ボタンをクリックします。
   1. **Q**&#x200B;と&#x200B;**APIレベル29**&#x200B;を選択します。
      * AVDマネージャーの初回起動時に、「Download the versioned API」を選択するように求められます。 「Q」リリースの横の「ダウンロード」リンクをクリックし、ダウンロードとインストールを完了します。
   1. 「**次へ**」ボタンをクリックします。
   1. 「**完了**」ボタンをクリックします。
1. **AVDマネージャー**&#x200B;ウィンドウを閉じます。
1. 上部のメニューバーで、**実行/設定を編集**&#x200B;ドロップダウンから&#x200B;**wknd-mobile.x.x**&#x200B;を選択します。
1. 選択した&#x200B;**実行/設定を編集**&#x200B;の横にある&#x200B;**実行**&#x200B;ボタンをタップします
1. ポップアップで、新しく作成した&#x200B;**[!DNL Pixel 2 API 29]**&#x200B;仮想デバイスを選択し、**OK**&#x200B;をタップします
1. [!DNL WKND Mobile]アプリがすぐに読み込まれない場合は、エミュレーターのAndroidホーム画面で&#x200B;**[!DNL WKND]**&#x200B;アイコンを探してタップします。
   * エミュレータが起動してもエミュレータの画面が黒のままの場合は、エミュレータのツールウィンドウの隣にある&#x200B;**power**&#x200B;ボタンをタップします。
   * 仮想デバイス内をスクロールするには、クリック&amp;ホールドしてドラッグします。
   * AEMからコンテンツを更新するには、上から更新アイコンまでプルダウンします。
を表示、リリースします。

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## モバイルアプリコード

この節では、AEM Content ServicesとJSON出力に最も相互に作用し、依存するAndroidモバイルアプリコードについて説明します。

読み込み時に、モバイルアプリは`HTTP GET`を`/content/wknd-mobile/en/api/events.model.json`にします。これは、モバイルアプリを駆動するコンテンツを提供するように設定されたAEM Content Servicesエンドポイントです。

イベントAPI(`/content/wknd-mobile/en/api/events.model.json`)の編集可能なテンプレートがロックされているので、Mobile AppはJSON応答の特定の場所で特定の情報を探すようにコード化できます。

### ハイレベルコードフロー

1. [!DNL WKND Mobile]アプリを開くと、`/content/wknd-mobile/en/api/events.model.json`にあるAEM Publishへの`HTTP GET`リクエストが呼び出され、コンテンツが収集されてモバイルアプリのUIに表示されます。
2. AEMからコンテンツを受け取ると、モバイルアプリの3つの表示要素、**ロゴ、タグ行、イベントリスト**&#x200B;のそれぞれがAEMのコンテンツで初期化されます。
   * AEMコンテンツをモバイルアプリの表示要素に連結するために、各AEMコンポーネントを表すJSONはJava POJOにマッピングされ、Android表示要素に連結されます。
      * 画像コンポーネントJSON→ロゴPOJO→ロゴImageView
      * テキストコンポーネントJSON→ TagLine POJO→ Text ImageView
      * コンテンツフラグメントリストJSON→イベントPOJO→イベントリサイクラービュー
   * *Mobile Appコードは、より大きなJSON応答内の既知の場所があるので、JSONをPOJOにマッピングできます。「image」、「text」および「contentfragmentlist」のJSONキーは、バッキングしているAEMコンポーネントのノード名によって決まります。 これらのノード名が変更されると、必要なコンテンツをJSONデータからソースする方法が分からないので、モバイルアプリが中断します。*

#### AEM Content Servicesエンドポイントの呼び出し

以下は、モバイルアプリの`MainActivity`のコードの要約です。このコードは、AEM Content Servicesを呼び出して、Mobile Appエクスペリエンスを駆動するコンテンツを収集する役割を持ちます。

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

`onCreate(..)` はモバイルアプリの初期化フックで、JSONを解析し、値を `ViewBinders` 要素にバインドする3つのカスタムを `View` 担当します。

`initApp(...)` が呼び出され、AEM Publish上のAEM Content Servicesエンドポイントに対してHTTPGETリクエストが行われ、コンテンツが収集されます。有効なJSON応答を受け取ると、JSON応答が各`ViewBinder`に渡されます。各は、JSONを解析し、モバイル`View`要素にバインドします。

#### JSON応答の解析

次に、`LogoViewBinder`を見てみましょう。これは簡単ですが、重要な考慮事項がいくつか浮かび上がります。

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

`bind(...)`の最初の行は、コンポーネントが追加されたAEMレイアウトコンテナを表すキー&#x200B;**:items→ root→ :items**&#x200B;を介してJSON応答を下に移動します。

ここから、画像コンポーネントを表す&#x200B;**image**&#x200B;という名前のキーに対してチェックが行われます（再び、このノード名→ JSONキーは安定していることが重要です）。 このオブジェクトが存在する場合は、Jackson `ObjectMapper`ライブラリを介して[カスタムイメージPOJO](#image-pojo)に読み込まれ、マッピングされます。 画像POJOについては、以下に説明します。

最後に、[!DNL Glide]ヘルパーライブラリを使用して、ロゴの`src`がAndroid ImageViewに読み込まれます。

AEM Content ServicesはJCRパス(例： `aemHost``/content/dam/wknd-mobile/images/wknd-logo.png`)を参照先コンテンツに追加します。

#### 画像POJO{#image-pojo}

オプションですが、Gsonなど他のライブラリで提供される[Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html)や同様の機能を使用すると、複雑なJSON構造をJava POJOにマッピングするのに役立ちます。ネイティブJSONオブジェクト自体を直接処理する必要はありません。 この単純なケースでは、`image` JSONオブジェクトの`src`キーを、`@JSONProperty`注釈を介して直接画像POJOの`src`属性にマップします。

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

イベントPOJOは、JSONオブジェクトから多くのデータポイントを選択する必要があり、この方法の利点は、単純な画像よりも多くあります。単に`src`が必要です。

## モバイルアプリのエクスペリエンスの調査

これで、AEM Content Servicesがどのようにネイティブのモバイルエクスペリエンスを引き起こすかを理解できたので、次の手順の実行方法を学習し、変更内容をモバイルアプリに反映させて確認できます。

各手順の後、モバイルアプリを引っ張って更新し、モバイルエクスペリエンスの更新を確認します。

1. **新しい[!DNL Event]コンテンツフラグメント**&#x200B;を作成して発行
1. **既存の[!DNL Event]コンテンツフラグメント**&#x200B;の非公開
1. **タグ行**&#x200B;に更新を発行

## これで完了です

**AEM Headless Tutorialは終了です。**

ヘッドレスCMSとしてのAEM Content ServicesとAEMについて詳しくは、Adobeの他のドキュメントおよび有効化に関する資料を参照してください。

* [コンテンツフラグメントの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [AEM WCMコアコンポーネントユーザーガイド](https://docs.adobe.com/content/help/ja/experience-manager-core-components/using/introduction.html)
* [AEM WCMコアコンポーネントコンポーネントライブラリ](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM WCMコアコンポーネントGitHubプロジェクト](https://github.com/adobe/aem-core-wcm-components)
* [AEM WCMコアコンポーネント — エキスパートにお問い合わせ](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [コンポーネントエクスポーターのコードサンプル](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
